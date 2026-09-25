---
title: "Modern Alternatives to PGP"
date: 2019-02-15T17:52:49Z
slug: "modern-alternatives-to-pgp"
author: "George Tankersley"
---

Did your last Yubikey just break? Perhaps you forgot an offline backup password. Maybe you're just tired of [living like a spy](https://gist.github.com/grugq/03167bed45e774551155) and never using smartphones. Whatever it is, you're here, and you're finally ready to [give up on PGP](https://blog.filippo.io/giving-up-on-long-term-pgp). That's great!

![](/content/images/2019/02/225357.jpg)

We're here to help!

No one was sending you encrypted emails anyway, so that's easy enough. But the most widespread uses of PGP are machine-oriented, for needs like package signing and local file encryption. I recently got into this again on a thread that mentioned [deprecating Go's OpenPGP package](https://github.com/golang/go/issues/30141) and people always ask the same question: if not PGP, then what?

The thing is, there can't be a direct modern alternative to PGP. It's a do-everything design that predates modern cryptographic engineering and hasn't worked out well. The "modern alternative" is to use a much more specific and much less configurable solution to your problem.

Here are the most common uses of PGP and their modern, focused alternatives.

## Signatures for OS or package updates

**Original need:** [Linux distributions](https://tutorials.ubuntu.com/tutorial/tutorial-how-to-verify-ubuntu#0) and many other software update mechanisms use PGP signatures to prevent malicious mirrors or network attackers from altering the contents of their packages.

**Modern alternative:** a lightweight signing tool like OpenBSD's [signify](https://https.www.google.com.tedunangst.com/flak/post/signify) or [minisign](https://jedisct1.github.io/minisign/), either of which you could quickly build in Go using [x/crypto/ed25519](https://godoc.org/golang.org/x/crypto/ed25519). I wrote one as a [party trick](https://github.com/gtank/sshsign) last month – it's less than 200 lines of code and that includes some silly key parsing tricks.

These tools are extremely simple and robust because they only support one algorithm, lack state or any notion of a keyring, and are usually decoupled from complex messaging formats (which should be a separate concern from your signature cryptography). As a bonus they're mostly compatible with each other because the keys are all [EdDSA](https://tools.ietf.org/html/rfc8032) keys. Need I even mention that EdDSA is much, much faster than RSA?

If you're a maintainer of self-serve packaging infrastructure or otherwise have more complex needs, you should take a look at [TUF](https://theupdateframework.github.io/). It's a modern toolkit meant to resist all kinds of update-related attacks that PGP signatures simply don't address. For an idea of how it works in practice, check out the designs for Python's package infrastructure ([PEP 458](https://www.python.org/dev/peps/pep-0458/), [PEP 480](https://www.python.org/dev/peps/pep-0480/)). The [Notary project](https://github.com/theupdateframework/notary) is a productionized and audited implementation.

## Encrypting data blobs

**Original need:** You want to store individual pieces of data without making their contents accessible to anyone else on your system. For some reason this isn't built into operating systems yet, so programs like [pass](https://www.passwordstore.org) use PGP.

**Modern alternative:** [nacl/box](https://godoc.org/golang.org/x/crypto/nacl/box) and [nacl/secretbox](https://godoc.org/golang.org/x/crypto/nacl/secretbox). These are general-purpose replacements for encrypting any kind of data. They support public-key and secret-key encryption, respectively, and both use secure modern modes and ciphers.

[Keybase](https://keybase.io) made a very solid attempt to specify nacl in a storable message format with [saltpack](https://saltpack.org), which has a good Go implementation using the above x/crypto package. It works as a nearly drop-in replacement anywhere you were using ASCII-armored PGP data.

## Sending files securely

**Original need:** You have files that you want to send to another person, but you don't want the data to be visible in transit or stored in the cloud. For this, folks often attach an encrypted ZIP file to an email.

**Modern alternative:** [magic-wormhole](https://magic-wormhole.readthedocs.io/en/latest/welcome.html). If you need to send files to another person securely, use this. It's based on a modern cryptographic technique called [PAKE](https://blog.cryptographyengineering.com/2018/10/19/lets-talk-about-pake/), which basically means it keeps your data safe in transit while giving an attacker no chance to crack the password. It generates those passwords for you, and they're short, one-time-use combinations of three English words – all your recipient needs to do is enter the words. There's nothing to remember long-term and no account information to lose in a data breach. Wormhole also works through firewalls and optionally supports Tor.

We live in the future. There are better options than emailing encrypted files around.

## Something else?

What's your lingering use case for PGP? Hit me up [on Twitter](https://twitter.com/gtank__) or send me [an email](mailto:blog@gtank.cc?subject=I'm still using PGP) and let's talk about getting you off PGP for good.
