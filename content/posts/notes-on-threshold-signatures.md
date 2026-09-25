---
title: "Notes on threshold signature schemes"
date: 2019-10-18T02:33:40Z
lastmod: 2019-10-18T18:28:48Z
slug: "notes-on-threshold-signatures"
tags:
  - "crypto"
  - "brain dumps"
author: "George Tankersley"
---

A threshold signature allows a subset *t* of a group of *n* possible signers to collectively produce a signature for the entire group. The simplest ones tend to use some [distributed key generation](https://en.wikipedia.org/wiki/Distributed_key_generation) ("DKG") based on [verifiable secret sharing](https://en.wikipedia.org/wiki/Verifiable_secret_sharing) ("VSS") to construct the keys and secret inputs to a [Schnorr signature](https://en.wikipedia.org/wiki/Schnorr_signature) in a distributed manner. There have been a lot of these, and recalling all the Feldman-this and Pedersen-that gets confusing. The following seem to be the core papers in the field of discrete-log threshold signatures:

- [Pedersen DKG](https://link.springer.com/content/pdf/10.1007%2F3-540-46766-1_9.pdf) (1991) was the first threshold scheme. It uses an earlier primitive called [Feldman VSS](https://en.wikipedia.org/wiki/Verifiable_secret_sharing#Feldman%E2%80%99s_scheme). It was later found to be vulnerable to malicious participants, but nevertheless remains a building block of subsequent schemes.
- [GJKR DKG](https://link.springer.com/content/pdf/10.1007/3-540-48910-X_21.pdf) (1999) describes the attack against the Pedersen DKG, which can be manipulated by participants to produce non-uniform keys. Their fix adds a new commitment round (which uses the alternative Pedersen VSS) to eliminate the ability of a participant to bias the results.
- [GJKR *Revisited*](https://pdfs.semanticscholar.org/642b/d1bbc86c7750cef9fa770e9e4ba86bd49eb9.pdf) (2003) describes a threshold Schnorr scheme that is secure even when using the original Pedersen DKG. The signature scheme isn't necessarily great, but it does save a round of communication vs GJKR99. Their main innovation is actually the proof techniques that explain when the problem with Pedersen DKG does not affect the security of the signature.
- [Stinson-Strobl](https://link.springer.com/chapter/10.1007/3-540-47719-5_33) (2001) is kind of off on its own, applying the GJKR99 result directly. It is more commonly cited now but GJKR03 appear to have been unaware of it.
- [This](https://www.sci-hub.tw/10.1007/s00145-006-0347-3) is some sort of helpful extended version of the GJKR work, combining GJKR99 and GJKR03 with good explanations of the design motivations. I have no idea what the context of this version was.
