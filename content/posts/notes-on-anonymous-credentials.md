---
title: "Notes on anonymous credentials"
date: 2020-03-26T20:06:25Z
slug: "notes-on-anonymous-credentials"
tags:
  - "crypto"
  - "brain dumps"
author: "George Tankersley"
---

Best anonymous credentials that don't use pairings:

Single show, no attributes, symmetric issuer: Privacy Pass; uses VOPRFs

Multi show, attributes, symmetric issuer: CMZ14 (original, <https://eprint.iacr.org/2013/516>)/CPZ19 (group element attribute variant, <https://eprint.iacr.org/2019/1416>); uses "algebraic MACs" and categorically supercedes U-Prove

Single show, attributes, publicly verifiable: Anonymous Credentials Light; uses an Abe signature variant

For multi-show with attributes *and* public verifiability, as far as I know you need to use pairings to get randomizable signatures. The best general credential of this type might still be the first real one, CL04 (<https://cs.brown.edu/people/alysyans/papers/cl04.pdf>) but probably now you'd instantiate it with Pointcheval-Sanders signatures (PS16, <https://eprint.iacr.org/2015/525> + PS18, <https://eprint.iacr.org/2017/1197> tweaked for better proofs) which allows for constant-size credentials.

I believe that brings you to the current frontier, where I am aware of some work that cranks the bells-and-whistles up to 11 (e.g. Coconut, <https://arxiv.org/pdf/1802.07344.pdf>) but not anything I'd call general-purpose usable or elegant yet.

If you need what are called "delegatable credentials", which allow you to anonymously encode issuer hierarchies like a certificate chain, that's a whole other line of research. Largely the same authors, but the schemes are heavy compared to the above "unlinkable MAC -> blind signature -> rerandomizable signature" progression.

There's a relatively parallel approach (again though, lots of the same names) that ditches the reliance on ZK proofs for malleable signature schemes that I've never really gotten into. Its main "things" appear to be delegation and selective disclosure of attributes. There have been several new schemes in this vein since the last time I read up on AC, but they all sound really similar... I don't know what the differences are or understand the approach tbh.

Probably the original proposal was "malleable signatures" from CKLM14 (<https://eprint.iacr.org/2013/179>) or "unlinkable redactable signatures" from CDHK15 (<https://eprint.iacr.org/2015/580>).

Then, recently:

Delegatable Attribute-based Anonymous Credentials from Dynamically Malleable Signatures <https://eprint.iacr.org/2018/340>

Delegatable Anonymous Credentials from Mercurial Signatures <https://eprint.iacr.org/2018/923>

Efficient Redactable Signature and Application to Anonymous Credentials <https://eprint.iacr.org/2019/1201>
