---
title: "Getting Started With Tor Development"
date: 2017-10-04T00:43:40Z
lastmod: 2018-08-20T17:17:50Z
slug: "tor-dev-101"
tags:
  - "tor"
author: "George Tankersley"
---

## Introduction

[Tor](https://en.wikipedia.org/wiki/Tor_%28anonymity_network%29) is an anonymity and censorship circumvention tool that uses a network of relays around the world to mask the origins and destinations of traffic. It's used by researchers, journalists, and activists all over the world, as well as by ordinary people who want to keep their internet use private.

There's a lot of writing about Tor's protocol or its use as a privacy tool, but not a lot of writing about Tor as a codebase. I've been a minor contributor to the project in the past, and through various [other](https://speakerdeck.com/gtank/solving-the-cloudflare-captcha-rwc2017) [projects](https://speakerdeck.com/filosottile/non-hidden-hidden-services-considered-harmful) I've had a lot of opportunity to spend time with the Tor code and specs.

Since it's rare to be able to do that without working for Tor, this is meant to be the start of an occasional series about parts of the code I don't see discussed much elsewhere.

First, though, we'll need to find and build that code.

## Building tor

Tor hosts all of its code, docs, and tooling at <https://git.torproject.org>. There is a ton of useful stuff in here, but for now we're looking for the core tor daemon (or "little-t tor", to distinguish it from Tor the overarching project) that lives in its own top-level repo:

`git clone https://git.torproject.org/tor.git`

Now, the README claims that all we need to do next is run

`sh autogen.sh && ./configure && make && make install`

but in practice we're missing dependencies. On Debian, the most basic set of dependencies I have to install are development headers for libevent, openssl, zlib, libcap, libseccomp, and libscrypt. YMMV.

`sudo apt-get install libevent-dev libssl-dev zlib1g-dev libcap-dev libseccomp-dev libscrypt-dev`

libcap, libseccomp, and libscrypt aren't strictly required, but they allow the tor daemon to use features it would otherwise not have access to. You can see places where this makes a difference by grepping the code for things like `HAVE_SCRYPT`, `HAVE_SECCOMP`, or `HAVE_SYS_CAPABILITIES_H`.

Finally, since we don't really care about building the manpages right now and asciidoc has a footprint far too large for my Chromebook's disk, we can omit them:

`./configure --disable-asciidoc`

So now we're ready to run `make` and get a tor binary. If you haven't changed anything else, it'll be at `./src/or/tor`. We can install it with `make install`, which will come in handy for the next section.

> **NOTE:** There are *tons* of other build options, and I'm not sure I understand them all well enough to tell you what a "production-ready" tor build would look like. This certainly is not one.

When you run tor without further configuration, it's just a client. It will automatically connect to the network and start a local SOCKS5 proxy that routes your traffic through Tor. But the tor binary is both a Tor client and potentially a Tor relay, all in one giant codebase, so in fact we have everything we need to build a local Tor network!

## Testing tor

Tor does have unit tests; you'll find them in `src/test` and they're not too hard to figure out. But Tor is a distributed system, and a tor binary alone isn't very useful for testing changes. By default it will talk to the real Tor network, which is a bit like testing in production—and you wouldn't *ever* do that, would you? Just for this reason, there's a tool called [chutney](https://gitweb.torproject.org/chutney.git) that lets us run a few relays locally, using whatever tor binary we have available.

First, we get chutney with

`git clone https://git.torproject.org/chutney.git`

Chutney is a magical project, so the only configuration it needs is where to find a tor binary. We can provide that in several ways:

- Having tor installed somewhere in our path, or
- Specifying the location of the 'tor' and 'tor-gencert' binaries with the environment variables `CHUTNEY_TOR` and `CHUTNEY_TOR_GENCERT`, respectively, or
- Running chutney from a tor build directory

Since we ran `make install` earlier, we're done! If not, we'll need to set some environment variables or run chutney from the build directory.

Once it found tor, we can tell chutney to set up the config files for a basic, minimal tor network:

`./chutney configure networks/basic`

This will generate identity keys and configuration files for the relays in your local network. Then, we can start the testing network with `./chutney start networks/basic`.

This is a fully-functioning, if small, tor network. We can see the status of our network with `./chutney status networks/basic`:

```
$ ./chutney status networks/basic
Using Python 2.7.13
test000a is running with PID 23439
test001a is running with PID 23442
test002a is running with PID 23445
test003r is running with PID 23448
test004r is running with PID 23451
test005r is running with PID 23454
test006r is running with PID 23457
test007r is running with PID 23460
test008c is running with PID 23463
test009c is running with PID 23466
10/10 nodes are running
```

Each node is a separate tor process using a directory under `net/` to store config and keys. The letters at the end of node names tell us what type of relay they are: 'a' for directory authorities, 'r' for relays, and 'c' for clients. We can see from the file `networks/basic` that this is the number and distribution of relays we'd expect for the basic network: three authorities, five relays configured as exits, and two clients.

```
# By default, Authorities are not configured as exits
Authority = Node(tag="a", authority=1, relay=1, torrc="authority.tmpl")
ExitRelay = Node(tag="r", relay=1, exit=1, torrc="relay.tmpl")
Client = Node(tag="c", client=1, torrc="client.tmpl")

# We need 8 authorities/relays/exits to ensure at least 2 get the guard flag
# in 0.2.6
NODES = Authority.getN(3) + ExitRelay.getN(5) + Client.getN(2)

ConfigureNodes(NODES)
```

We can verify it's working correctly with `./chutney verify networks/basic`.

This is a basic functionality test, but when we're developing we need more: we need to interact directly with the relays. What if we want to get an overview of the network we're running, to check that a relay flag is being applied? With chutney, it couldn't be any easier—the relays are all bound to localhost! By default chutney configures directory ports to start at 7000. Since we happen to have a directory authority as the first relay, that means we can get a status document with:

`curl http://127.0.0.1:7000/tor/status-vote/current/consensus`

This will return a signed document that fully describes all of the tor relays in our network, including their identity keys, addresses, open ports, supported protocol versions, and any flags the network authorities have applied to them.

So chutney is a handy tool we can use to poke at our own little Tor network, to experiment with features or validate any changes we've made. It's generally considered impolite to run experimental tor changes on the real network; try it out in chutney first!

When we're done, `./chutney stop networks/basic` will shut everything down.

## Contributing to tor

Now that you know how to build and test tor, what next?

The Tor Project uses a Trac instance at <https://trac.torproject.org> to manage development. Most discussion of new design proposals happens on the tor-dev [mailing list](https://lists.torproject.org/cgi-bin/mailman/listinfo/tor-dev/), which is an extremely low-volume mailing list. Regular team meetings and real-time discussion live on an IRC channel specifically for developing Tor projects, #tor-dev on OFTC.

But really, the best place to get started hacking on tor is in the git repo! The directory `doc/HACKING/` has pointers to everything you'll need to get started. Another repo called ["torguts"](https://gitweb.torproject.org/user/nickm/torguts.git/) contains a work-in-progress detailed overview of the code organization and development conventions.

Beyond that, Trac has an ["easy" category](https://trac.torproject.org/projects/tor/query?status=accepted&status=assigned&status=needs_review&status=new&status=reopened&order=priority&keywords=~easy&col=id&col=summary&col=component&col=status&col=type&col=priority&col=milestone&report=30) of bugs if you're looking for something to get started with.

Next time I'll cover something more in-depth, and in the meantime—why not try fixing an easy tor bug? And if you're interested in more cryptography, privacy, or projects like Tor you should [follow me on Twitter](https://twitter.com/gtank__).
