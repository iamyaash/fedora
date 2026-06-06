---
date: '2026-06-04T13:22:37+05:30'
draft: true
title: 'A Better Commit Page for Forgejo: Fixes, Tweaks, and Redesign'
author: "Yashwanth Rathakrishnan"
ShowToc: true
TocOpen: true 
ShowReadingTime: true
ShowBreadCrumbs: true
ShowCodeCopyButtons: true
---

# What's wrong with the old design?
For starters it's **old, outdated** and most importantly it was **broken on mobile view** layouts. The commit page layout has never been touched after it's final design and only minor fixes and changes has been made. The commit page was more developer-friendly than user friendly. New users often find themselves struggling understand what's being show in the commit page. (Me included in the early days). In my opinion, the reason is everyone is used to GitHub, and GitLab and they both share a similar yet distinguisable user interface. But seeing a different ui choice in Forgejo/Codeberg is often difficult to understand the design.

Here, an issue was raised to make changes to the commit view table to group the commit view.
> [feat: indicate commit order in PR view #5178](https://codeberg.org/forgejo/forgejo/issues/5178)

# Why I decided to work on this?
Clearly indicating that we need a fresh look on commit page view. So, I volunteered to take on this whole revamp, during April, 2025. When I was being mentored by Tomas Herka, Justin Wheeler and Akashdeep Dhar as part of my Fedora Fellowship. At first, I brought this topic to Tomas Hercka, that I wanted to take on responsibility of working on frontend side of things and that's when he suggested me to work on this commit layout issue on Forgejo. But at first, I wanted to work on Fedora's Forgejo (now called as Fedora Forge), but Tomas Hercka suggested me to work on Forgejo itself, since we don't have much new contributors to work Forgejo, since the changes made to Forgejo are going to be reflected in Fedora Forge anyway. This decision was clearly beneficial for me, because I got the opportunity to connect with Forgejo community people and I got to learn how to work on Forgejo with the help of others teaching me.
As the time goes on, I gotten very active within the Forgejo community, asking for doubts, guidance, feedback and more.

{{< figure src="/fedora/posts/forgejo/feat/img/commit-issue-5178.png" title="" caption="" alt="comment made on issue #5178" >}}

So I eventually, started to get my hands on the revamp of the new design after making a comment on [this issue](https://codeberg.org/forgejo/forgejo/issues/5178).

# What's changed in the new design?
I would say it's a lot, since we revamped the whole UI of the commit page, and we have also fixed a couple of bugs while we're at it.

