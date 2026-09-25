---
title: "Humanist social networking"
date: 2019-08-19T19:56:00Z
lastmod: 2020-04-09T21:28:54Z
slug: "humanist-social-networks"
tags:
  - "social media"
author: "George Tankersley"
---

Despite their name and ostensible purpose, social networks aren't very human. To companies, engineers, and advertisers, the emphasis has been on *network*, not *social*. We flatten users into profiles and communities into graphs. While it's certainly convenient for databases, it's an utter failure at capturing the nuances of social interaction—when you force people to interact in inhuman ways, you can't be surprised if you get inhuman behavior.

How do we fix it? I'll tell you up front that I don't have answers. It's looking pretty grim out there. From here in 2019, it looks like may not get to keep both global social networks and a functioning society.

What I do have are thoughts about social networking that treats its users as human and accounts for some of the realities of human lives. Maybe that's enough to get started.

## Basic principles

A humanist social network is one that

**Offers a UI for rich consent***.* Some platforms think we have this already, but they barely scratch the surface. Online social connections should mirror the fine-grained options we have in person. Social permissions should be, at minimum:

- *Time-limited*. It's a common principle of modern security design that permanent privileges should not exist. If you want someone to have continued access, renew it. Platforms could even automate that in an opt-in fashion. In real life, though, no one grants permanent access to anyone but their closest companions. Perhaps you want to friend or follow someone for the duration of a professional event, or only during a job hunt. Situational or temporary relationships exist in people's lives, and the choice of a permanent connection or none at all is completely inhuman. You invite friends over for dinner, not to live on your couch and watch your every move.
- *Granular*. I should be able to separate personal friends from work friends, and family from both. People present different identities in different parts of their lives. To force a unified audience on someone is to offer them a choice between uncomfortably blurred boundaries and fleeing your platform for one that respects this reality.
- *Undetectable*. There are real social consequences to not sharing with others. Family might be offended, a boss might suspect you're hiding something. A block can look like engagement to an abuser just as much as a response would. Twitter's blocks are a prime example of badness here: they're so visible that people brag about getting blocked by prominent figures like it's a kind of opposition trophy. These features must not be implemented in such a way that users are punished for using them.

Another critical tool for social consent is effective blocking, but I'll cover that in #4.

**2. Respects privacy by default**. Users must have total discretion over when, how, and how much they and their actions are surfaced to anyone else. I say "discretion" rather than "control" because these things ARE complex, and the platform should probably handle most of the details most of the time based on basic user preferences. The median user may never care, but you have to allow genuine agency, since those who do care may care a great deal. There should be:

- *No unintuitive side channels* like Facebook's "People You May Know" uses. Correlations like "the two of you once used the same WiFi network" or "someone (perhaps your therapist) has both of you in their contacts list" might as well be dark magic to anyone who isn't absolutely steeped in tech. Even users who know the mechanisms can't avoid them by any reasonable measure. What will you do about it, never use WiFi? Give an individualized phone number to everyone you ever meet? There's no way for a user to "just be careful" here, especially once machine learning is involved, so the onus is on the platform not to be abusive.
- *No surprises* like Twitter's sudden shift to treating favorites as "soft retweets." If something has previously been private or quasi-private then you must not violate users' reasonable expectation that it will remain that way. Add a new feature rather than changing the behavior of the old one. Better yet, don't force people into dubiously consensual sharing to boost engagement.

**3. Acknowledges coercion**. Most people's threat models aren't about hackers or some nebulous oppressive state, but people close to them in their lives. Family, partners, colleagues—actual people who, for whatever reason, you can't just say no to or walk away from. A human-centered social app should understand this, and offer tools for users to selectively hide or disclose elements of their activity. The user interface can't be a single flat trust boundary anymore, particularly in the case of messaging.

One case to consider is behavior that's technically authorized, but suddenly and drastically different than what the user normally does. Anomaly detection saves us from credit card fraud every day. Could it also prevent an abusive partner from going through your entire message history when you accidentally leave your phone unlocked? This is space where platforms have *much* more leverage than individual users. A person who takes measures against this behavior, like frequently deleting messages, is seen to be resisting; platforms get to set expectations. A well-implemented (i.e. not obvious, or simply on-by-default) anti-coercion feature isn't accountable to anyone in a person's life. It's just "how things work."

**4. Removes or limits global addressability**. This is tough one, because the benefits of being able to reach out to the entire world are immense, and can be literally life-saving for marginalized folks in isolated or abusive situations. However, the benefits of the world being able to reach you, without your consent or intention, are at best debatable (ask anyone who's been on the receiving end of more internet attention than they expected). These days, it's clear that a responsibly-built social network needs to give users the ability to stop hearing from or being seen by anyone they don't approve. This helps mitigate everything from trollstorms to your ex-partner creating new accounts faster than you can block them. The burden of unwanted communication—and it is a burden—shouldn't fall on users when your platform could handle it for them.

## Toward better social networks

If we've learned anything from the last few years, it's that the current model of social networking can't cope with putting everyone on the planet together in a single giant room. It's failed in horrible ways, with consequences that run from ruining individual people's lives to destabilizing the entire concept of liberal democracy.

In that same span of time, a lot of functional communities have migrated off open platforms like Twitter and into closed ones like Slack, Facebook, or Telegram groups. The invite-only nature and access control seems to allow the quality of conversation I remember from pre-social-media communities to return. And maybe that's the stable solution here: small groups, unified by affinity or shared values, who can choose who to let in and who to keep out.

If that's the case, I mourn the loss of an open internet. I owe my entire career and a lot of friends to the fact that the open source community welcomed me without invitation or pedigree.

But maybe we don't have to retreat from the public sphere. We could build better social networks. We could design systems that take real humans into account, rather than what's convenient to store in a database.

At this point, what have we got to lose?
