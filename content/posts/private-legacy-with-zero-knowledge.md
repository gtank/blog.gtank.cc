---
title: "Adding privacy to legacy protocols with zero-knowledge proofs"
date: 2019-08-18T22:24:19Z
lastmod: 2019-08-18T22:26:33Z
slug: "private-legacy-with-zero-knowledge"
tags:
  - "crypto"
author: "George Tankersley"
---

Last January, Adam Langley did a really cool thing. He [grafted zero-knowledge proofs](https://www.imperialviolet.org/2019/01/01/zkattestation.html) onto an already-deployed, non-upgradable hardware system, thereby gaining privacy that the original design never allowed.

I am VERY EXCITED ABOUT THIS. It's an existence proof for all kinds of amazing things.

## The problem

The specific problem here is that some websites want to know what type of [security key](https://en.wikipedia.org/wiki/Universal_2nd_Factor) you're using. Security keys support this by showing the site a signed "attestation certificate", which sounds fine...until you consider the details. The attestation key is burnt into the device at manufacture time, and will be the same for every attestation. Attestation certificates are [supposed to be issued in large batches](https://www.chromium.org/security-keys), but nothing holds vendors to it. Even if vendors are honest and competent, 100k possible identities isn't very many in the face of [modern tracking techniques](https://panopticlick.eff.org/). And worse: each site gets to make very granular decisions about which brands and batches of keys to trust, which goes against the idea of a standards-based web.

Thus, as specified, attestation provides a supercookie that also attacks the promise of the open web. Since we (apparently) want to keep this feature for its potential security benefits, we need to find some way to do it without giving every site we authenticate to a powerful new way to track and constrain us.

## The solution?

As everyone knows, the solution to any cryptography problem is **more cryptography**. New crypto offers a lot of ways around this problem. With [randomizable signatures](https://eprint.iacr.org/2015/395) you could change the public key each time you make a signature. Full-on [anonymous credentials](https://eprint.iacr.org/2012/298) let you prove your authorization without ever providing a trackable identity. Adam's solution lies somewhere in between, and is called a *proof of knowledge of a signature*: he suggests having your browser make a zero-knowledge proof that it's seen a valid attestation signature from one of a list of acceptable public keys. You just show the proof to the website. You don't reveal anything else, especially not the signature itself (which would be trackable). It sounds wild but it's a pretty standard thing in cryptography literature.

BUT. Of course there's a but—I said "new cryptography" up there. Until about 2013, such proofs required [exotic](https://en.wikipedia.org/wiki/Signatures_with_efficient_protocols) [signature](https://cs.brown.edu/people/alysyans/papers/cl04.pdf) [algorithms](https://eprint.iacr.org/2006/184). Security keys don't do those. They do [ECDSA](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm) with the NIST P256 curve. Even worse, they're widely deployed consumer hardware! Upgrades are impossible.

## Making it work

Enter Bulletproofs. [Bulletproofs](https://medium.com/@cathieyun/building-on-bulletproofs-2faa58af0ba8) are a new technique for making [zero-knowledge proofs](https://zkp.science/) of general computations, provided you can express them in terms of a computational model called equivalently an *arithmetic circuit* or a *rank-1 constraint system*. Like [zk-SNARKs](https://z.cash/technology/zksnarks/), Bulletproofs allow you to make efficient proofs about an extremely wide variety of algorithms.

Both Bulletproofs and SNARKs are concretely built from an elliptic curve and a *circuit.* Elliptic curves have both a base [field](https://en.wikipedia.org/wiki/Finite_field), in which they are defined, and a scalar field, which is related to the number of points on the curve. The circuit is defined over the curve's scalar field and encodes some relation between inputs and outputs. The circuit's operations can be almost anything. For instance, they can encode an ECDSA signature check.

Thus we have an outer curve—the one that implements the Bulletproofs—and an inner curve—the one that we're making proofs about—that is said to be *embedded* in the circuit. Usually you define your outer curve to make the Bulletproofs efficient, then you construct an entirely new embedded curve over its scalar field. Since the outer curve's scalar field arithmetic is the "native integer" type inside the circuit, having it be the base field for your embedded curve makes the embedded curve operations very efficient inside the circuit. This is what Zcash did with its latest protocol upgrade: [BLS12-381](https://electriccoin.co/blog/new-snark-curve/) is the outer curve and [Jubjub](https://z.cash/technology/jubjub/) is the embedded curve defined over its scalar field.

Unfortunately, we can't do that here. P256 is standardized, so we can't use a different curve while remaining compatible with security keys. So Adam used a technique called [complex multiplication](https://arxiv.org/abs/0712.2022) to **work backward** from P256 to a set of Bulletproofs parameters that can efficiently embed the P256 curve—that is, he constructed an outer curve whose scalar field is the same as the P256 base field.

Then he rendered ECDSA validation as an arithmetic circuit. The main determinant of circuit performance is the number of constraints it contains and this ECDSA circuit contains about 17K constraints. That's a decent result, though (while I couldn't do it personally) [significantly](https://www.zfnd.org/zcon/0/workshop-notes/Zcon0%20Circuit%20Optimisation%20handout.pdf) [better](https://github.com/zcash/zcash/issues/3425) circuit techniques exist. The gist of them is that porting classically efficient math to circuits isn’t optimal. The cost model in a circuit is very different if you’re used to optimizing curves; for instance, affine formulas are often more efficient than projective ones and field addition is so cheap that it can make sense to use [Toom-12](https://en.wikipedia.org/wiki/Toom-Cook_multiplication).Yes, [twelve](https://github.com/zcash/zcash/issues/3425#issuecomment-424357821).

Anyway. IT WORKS. THIS IS MINDBLOWING. Using his parameters, we can make efficient zero-knowledge proofs about any system that uses P256 like we just did by turning security key signatures into Bulletproofs. Similar techniques would apply to other legacy algorithms. ***This means we can build privacy-preserving systems on top of previously deployed non-private ones, without breaking compatibility!***

If we've learned anything from TLS over the years, it's that not breaking old systems is critical to real-world rollouts. Most of our deployed crypto algorithms (even security keys themselves) were standardized back when this type of zero-knowledge proof was an academic fantasy. That's changed rapidly, and Adam's proof of concept shows that we aren't bound by what seemed feasible back in 2005.

Get. pysched.
