---
title: "Efficient Private Contact Search"
date: 2019-04-22T16:08:39Z
lastmod: 2019-04-23T17:49:55Z
slug: "private-contact-search"
tags:
  - "crypto"
author: "George Tankersley"
---

Recently, a [friend](https://twitter.com/filosottile) and I indulged in the very normal spring weekend activity of discussing the [Signal contact discovery problem](https://signal.org/blog/contact-discovery) in the park.

The contact discovery problem is this: a service holds a list of all registered users, and an individual user has an address book. The user wants to learn which of their contacts is also registered for the service without leaking the contents of their address book to the service, and the service wants to provide that information without publishing a global list of users to anyone who doesn't already know them. The typical app's solution to this is for the user to upload their entire address book to the service.

For services that value privacy, like Signal, that's unacceptable. So Signal tried a lot of potential techniques to solve this before they finally settled on taking the uploads, but promising not to peek by [using SGX](https://signal.org/blog/private-contact-discovery/). But as idle weekend park cryptographers, we thought it would be fun to revisit some of the discarded possibilities.

We noticed that one of the options they dismiss as insufficiently scalable, which they called an "encrypted [Bloom filter](https://en.wikipedia.org/wiki/Bloom_filter)", is very similar to the first version of our earlier [Privacy Pass](https://privacypass.github.io/) scheme. In that version, we also used blind RSA signatures, and like Signal we determined that they weren't suitable for mass deployment since they're slow to calculate and quite large on the wire (~2kb per signature). For Privacy Pass we solved that problem by replacing blind RSA with a new primitive called an *oblivious pseudorandom function* ("OPRF"), obtaining roughly an order-of-magnitude performance improvement and hauling the whole idea right into the realm of practicality. So why not try the same thing here?

Since this idea is a fairly practical example of *private information retrieval*, which is often considered to be impractical, I realized our solution might be generally interesting enough to write down.

Therefore, our form of private set queries works as follows.

## The Scheme

We assume a prime-order group with a standard generator G and a deterministic map from bitstrings to group elements we'll call `HashToGroup`. I recommend the [ristretto255 group](https://ristretto.group) for practical implementations.

First, the server samples a blinding factor `k` randomly from the scalar field.

Then for each registered phone number, the server will:

1. Calculate `T = HashToGroup(number)` such that the discrete log `T/G` is unknown.
2. Blind `T` to produce `U = kT`.
3. Add `U` to a Bloom filter tuned for a low false positive rate.

Then, the client downloads this Bloom filter.

To query the Bloom filter, the client

1. Samples blinding factor `b` randomly from the scalar field.
2. Calculates `T = HashToGroup(number)`
3. Blind `T` to produce `V = bT` using the blinding factor `b`.
4. Sends `V` to the server. Subject to optional authentication, rate limiting, etc the server replies with `W = kV`.
5. Unblinds `W` by inverting the blinding factor.

If the numbers match then the user will arrive at the same element U as the server included in the Bloom filter via the following derivation:

```
(1/b)W 	= (1/b)(kV)
	= (1/b)(kb)T
	= (1/b)(b)(k)T
	= kT
	= U
```

Arriving at the same group element allows the client to query the Bloom filter!

However, at no time does the server publish a list of their users (the Bloom filter entries are masked by k) nor does the client reveal the contents of their queries (which are masked by b).

The best part is, assuming we're using elliptic curve groups, every quantity involved in the cryptography is about 10x smaller/faster/better than the blind RSA version. These operations are also potentially batchable.

## Attacks

As stated in the problem, we want to achieve two things here. First, the service should not learn the contents of the user's address book in the course of the query (it's a *different* hard problem to prevent the server from learning your contacts based on who you talk to). Secondly, the users should not learn the entire list of registered users unless they know all of their contact info already.

The first concern is handled in the scheme itself: the user does local queries on blinded contact list entries. The plain text of the contact information never leaves the user's client. The blinded values that the server receives can't be linked to the hashed phone numbers.

The second concern is a little harder. On the face of it, the user only receives blinded Bloom filter entries, and they can't make queries without the cooperation of the server. But for a space as small as the set of possible phone numbers a client could, over time, simply brute force all possible queries. To prevent these enumeration attacks, the server should apply prudent rate-limiting and rotate its blinding key on a regular basis. If the Bloom filter and blinding changes it becomes much harder for a maliciously enumerating user to ever be sure they have the whole list. In fact, a server with sufficient storage or compute could even have per-user keys to prevent multi-user collusion.

## Disclaimer

This was a weekend park thought! We're pretty sure it works, but if you're interested in using it or something like it for real, let's talk. Hit me up at [blog@gtank.cc](mailto:blog@gtank.cc) or [on Twitter](https://twitter.com/gtank__).

## Acknowledgements

Thanks to the incomparable [Daira Hopwood](https://twitter.com/feministPLT) who found an offline bruteforce vulnerability in the first published version of this protocol, and to Sean Devlin who pointed out a conceptual error in the additive masking variant.
