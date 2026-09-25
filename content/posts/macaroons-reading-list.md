---
title: "A Macaroons Reading List"
date: 2018-12-14T03:47:26Z
lastmod: 2020-03-26T20:12:09Z
slug: "macaroons-reading-list"
tags:
  - "crypto"
author: "George Tankersley"
---

Macaroons are one of my favorite cryptographic constructions. They were almost the first one I really understood, and they [heavily influence my designs](https://privacypass.github.io/) for anything involving authorization.

There's a lot to love about macaroons. They're elegant and fast. They're secure and easy to reason about. They offer new capabilities over what was there before while sacrificing nothing. They are the anti-JWT, and they are *tragically underused*.

A macaroon is a bearer token consisting of a list of statements and an [HMAC](https://en.wikipedia.org/wiki/HMAC) (a type of secret-keyed hash) of the whole list. The statements (called "caveats") are restrictions on what the bearer can do. They can be anything, like "must be logged in", "not after next Tuesday", "once per hour", "read but not post", or "can only see the /catpics directory". When a server protecting some resources sees a macaroon it issued, it can check the HMAC to see that the list is valid, then compare the restrictions to the bearer's request.

The magic of macaroons is that the *bearer* can extend them. The HMAC key for each new layer is the HMAC output of the layer that came before it, so anyone who holds a valid macaroon knows the secret necessary to add additional caveats. This is secure because good cryptographic hashes (like SHA2) can't be reversed. Bearers can't retrieve the prior hash outputs they'd need to create a macaroon with fewer caveats, so they can't increase their own privileges above those of the first macaroon they received. Since only the original issuer knows the secret key for the first layer(s), they get to decide the baseline permissions of users in the system.

But any bearer of a macaroon can *attenuate* the macaroon to further restrict downstream bearers. For instance, you might want to let an archival service read your GitHub repos without being able to make commits. To do that now, you have to sign in again for the archiver app with specially restricted permissions (via the rather-complex protocol [OAuth](https://developer.github.com/apps/building-oauth-apps/)) . With macaroons, you'd just add a read-only caveat and pass it along. You wouldn't even have to involve GitHub.

Macaroons are great for systems that follow the [principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege), or distributed systems where asking the centralized (or worse, third-party) auth provider to mint tokens all the time would constitute a single point of failure. Plus, these things are blazing fast because hashes are so much simpler than the public-key signatures or encryption used in other auth token designs.

That's it! The world doesn't need another detailed macaroon explainer. Instead, I put together this reading list that will take you from here to the current state of the art:

### References

- The original Google paper that no one fully implements. It's genius though. <https://ai.google/research/pubs/pub41892>
- A blog explaining the context in which macaroons exist, with comparisons <https://latacora.singles/2018/06/12/a-childs-garden.html>
- A blog explaining macaroons themselves in great detail  <http://evancordell.com/2015/09/27/macaroons-101-contextual-confinement.html>
- A short talk by [Tess Rinearson](https://twitter.com/@_tessr) of Chain about the perils of using macaroons without considering their slightly weird UX implications <https://www.youtube.com/watch?v=MZFv62qz8RU>
- The state of the art in macaroon-esque tokens, CATs, which are currently in use at Facebook <https://eprint.iacr.org/2018/413.pdf>
