---
date: '2026-06-04T13:22:37+05:30'
draft: true
title: "Forgejo's New Commit Page Design Is Here"
author: "Yashwanth Rathakrishnan"
ShowToc: true
TocOpen: true 
ShowReadingTime: true
ShowBreadCrumbs: true
ShowCodeCopyButtons: true
---

# What's wrong with the old design?
For starters it's **old, outdated** and most importantly it was **broken on mobile view** layouts. The commit page layout has never been touched after it's final design and only minor fixes and changes has been made. The commit page was more developer-friendly than user friendly. New users often find themselves struggling understand what's being shown in the commit page. (Me included in the early days). In my opinion, the reason is everyone is used to GitHub, and GitLab and they both share a similar yet distinguisable user interface. When seeing a different ui choice in Forgejo/Codeberg is often difficult to understand the design.

Here, an issue was raised to make changes to the commit view table to group the commit by date.
> [feat: indicate commit order in PR view #5178](https://codeberg.org/forgejo/forgejo/issues/5178)

# Why I decided to work on this?
I volunteered to take on this whole revamp, during April, 2025. When I was being mentored by [Tomáš Hrčka, Justin Wheeler and Akashdeep Dhar as part of my Fedora Fellowship](https://fedoraproject.org/wiki/User:Thisisyaash#Fellowship_-_Red_Hat). At first, I brought this topic to Tomáš Hrčka, that I wanted to take on responsibility of working on frontend side of things and that's when he [suggested me to work on this](https://codeberg.org/forgejo/forgejo/issues/5178) commit layout issue on Forgejo. But at first, I wanted to work on Fedora's Forgejo (now called as Fedora Forge), but Tomas Hercka suggested me to work on Forgejo itself, since we don't have much new contributors to work on Forgejo, since the changes made to Forgejo are going to be reflected in Fedora Forge anyway. This decision was clearly beneficial for me, because I got the opportunity to connect with Forgejo community people and I got to learn how to work ron Forgejo with the help of others teaching me.
As the time goes on, I gotten very active within the Forgejo community, asking for doubts, guidance, feedback and more.

{{< figure src="/fedora/posts/forgejo/feat/img/commit-issue-5178.png" title="" caption="" alt="comment made on issue #5178" >}}

Eventually, I got my hands on the revamp of the new design after making a comment on [this issue](https://codeberg.org/forgejo/forgejo/issues/5178) and started working on it right away!.

---

# What's changed in the new design?
I would say it's a lot, since we revamped the whole UI of the commit page, and we have also fixed a couple of bugs while we're at it.

At first, the requirement is to either add feature to order the commits since the commits list were being rendered inside the table that made it convinient for adding this commit order functionality. However, this functionality later seemed unnecessary and later clarifying it with the Forgejo folk from [Matrix channel](https://matrix.to/#/#forgejo-ui:matrix.org), we decided to entirely revamp the whole commit list.

## Current Design

### Desktop Layout (as of Jun 2026)
{{< 
    figure src="/fedora/posts/forgejo/feat/img/forgejo-current.png"
    title="Current Deskop Layout of Forgejo Commit Page"
    alt="current forgejo commit design (as of Jun, 2026)" 
>}}
This is the current design of the commit list and the commits list are being rendered inside the table. It looks good but it's broken on mobile layout and glitches that will occure when trying change the size of browser window.

### Mobile Layout (as of Jun 2026)
{{< 
    figure src="/fedora/posts/forgejo/feat/img/forgejo-current-mobile.png" 
    title="Current Mobile Layout of Forgejo Commit Page" 
    width="50%"
    alt="current forgejo commit design (as of Jun, 2026)" >}}

There's almost no proper mobile responsive css were written, also the ones that were written just helps the patch the glichtes kkeeping it from breaking or overlapping, and the table just shrinks horizontally when the windows size get shrinked. Additionally, we also loose the shabox along with it when the windows size is shrinked. This is what you see on an iPhone 14's screen.

## New Design
### Desktop Layout Ideas
{{< 
    figure src="/fedora/posts/forgejo/feat/img/redesign-forgejo-early.png" title="Early Design"
    caption="This is the current design of the commit list and the commits list are being rendered inside the table. It looks good but it's broken on mobile layout and glitches that will occure when trying change the size of browser window."
    alt="current forgejo commit design (as of Jun, 2026)"
>}}

{{< 
    figure src="/fedora/posts/forgejo/feat/img/redesign-forgejo-later.png" title="Refined Design"
    caption="This is the current design of the commit list and the commits list are being rendered inside the table. It looks good but it's broken on mobile layout and glitches that will occure when trying change the size of browser window."
    alt="current forgejo commit design (as of Jun, 2026)"
>}}


### Finalized Desktop Layout
{{< 
    figure src="/fedora/posts/forgejo/feat/img/final-forgejo-desktop.png" title="Current Deskop Layout of Forgejo Commit Page"
    alt="current forgejo commit design (as of Jun, 2026)"
>}}
This is the current design of the commit list and the commits list are being rendered inside the table. It looks good but it's broken on mobile layout and glitches that will occure when trying change the size of browser window.
### Mobile Layout Ideas
{{< 
    figure src="/fedora/posts/forgejo/feat/img/mobile-redesign-forgejo-early.png" title="Early Design"
    caption="This is the current design of the commit list and the commits list are being rendered inside the table. It looks good but it's broken on mobile layout and glitches that will occure when trying change the size of browser window."
    width="60%"
    alt="current forgejo commit design (as of Jun, 2026)"
>}}

{{< 
    figure src="/fedora/posts/forgejo/feat/img/mobile-redesign-forgejo-later.png" title="Refined Design"
    caption="This is the current design of the commit list and the commits list are being rendered inside the table. It looks good but it's broken on mobile layout and glitches that will occure when trying change the size of browser window."
    width="60%"
    alt="current forgejo commit design (as of Jun, 2026)"
>}}

### Finalized Mobile Layout
{{< 
    figure src="/fedora/posts/forgejo/feat/img/final-forgejo-mobile.png"
    title="Current Deskop Layout of Forgejo Commit Page"
    alt="current forgejo commit design (as of Jun, 2026)" 
>}}
This is the current design of the commit list and the commits list are being rendered inside the table. It looks good but it's broken on mobile layout and glitches that will occure when trying change the size of browser window.
