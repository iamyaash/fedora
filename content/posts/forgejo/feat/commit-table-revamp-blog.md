---
date: '2026-06-09T19:43:08+05:30'
draft: false
title: "Forgejo's New Commit Page Design Is Here"
author: "Yashwanth Rathakrishnan"
ShowToc: true
TocOpen: true 
ShowReadingTime: true
ShowBreadCrumbs: true
ShowCodeCopyButtons: true
---

# What's Wrong with the Old Design
For starters, it's **old** and **outdated**, and most importantly, it was **broken on mobile view** layouts. The commit page layout has never been touched after its final design; only minor fixes and changes have been made. The commit page was more developer-friendly than user-friendly (IMO). New users often find themselves struggling to understand what's being shown on the commit page (me included in the early days). In my opinion, the reason is that everyone is used to GitHub and GitLab, and they both share a similar yet distinguishable user interface. When seeing a different UI choice in Forgejo/Codeberg, it is often difficult to understand the design.
Here, an issue was raised to make changes to the commit view table to group the commits by date.
> [feat: indicate commit order in PR view #5178](https://codeberg.org/forgejo/forgejo/issues/5178)

# Why I Decided to Work on This
I volunteered to take on this whole revamp during April 2025, when I was being mentored by [Tomáš Hrčka, Justin Wheeler, and Akashdeep Dhar as part of my Fedora Fellowship](https://fedoraproject.org/wiki/User:Thisisyaash#Fellowship_-_Red_Hat). At first, I brought this topic to Tomáš Hrčka, saying that I wanted to take responsibility for working on the frontend side of things. That's when he [suggested I work on this](https://codeberg.org/forgejo/forgejo/issues/5178) commit layout issue on Forgejo.

At first, I wanted to work on Fedora's Forgejo (now called Fedora Forge), but [Tomáš Hrčka](https://accounts.fedoraproject.org/user/humaton/) suggested I work on Forgejo itself since we don't have many new contributors to work on Forgejo. The changes made to Forgejo will be reflected in Fedora Forge anyway. This decision was clearly beneficial for me because I got the opportunity to connect with Forgejo community members, and I learned how to work on Forgejo with the help of others teaching me.

As time went on, I gotten very active within the Forgejo community, asking for doubts, guidance, feedback, and more.

{{< figure src="/fedora/posts/forgejo/feat/img/commit-issue-5178.png" title="" caption="" alt="comment made on issue #5178" >}}

Eventually, I got my hands on the revamp of the new design after making a comment on [this issue](https://codeberg.org/forgejo/forgejo/issues/5178) and started working on it right away!.

---

# What's Changed in the New Design
>  [feat(ui): commit view redesign for pull request page #7948](https://codeberg.org/forgejo/forgejo/pulls/7948) 

I would say it's a lot, since we revamped the whole UI of the commit page, and we also fixed a couple of bugs while we were at it.

At first, the requirement was to add a feature to order the commits, since the commits list was being rendered inside the table, which made it convenient for adding this commit order functionality. However, this functionality later seemed unnecessary, and after clarifying it with the Forgejo folks on the [Matrix channel](https://matrix.to/#/#forgejo-ui:matrix.org), we decided to entirely revamp the whole commit list.

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

There is almost no proper mobile-responsive CSS written, and the CSS that does exist only helps patch the existing glitches, keeping the layout from breaking or overlapping. The table simply shrinks horizontally when the window size is reduced. Additionally, the SHA box is also lost when the window size is reduced. This is what you see on an iPhone 14 screen.

## New Design
### Desktop Layout Ideas
{{< 
    figure src="/fedora/posts/forgejo/feat/img/redesign-forgejo-early.png" title="Early Design"
    caption="This was the design I thought of in the early phase, grouping the commits by date with each having its own border. Also, I highlighted the SHA box with a solid color instead of the neon theme. Since this is a mock design, it clearly needs improvement, and this design looks a bit not user-friendly."
    alt="current forgejo commit design (as of Jun, 2026)"
>}}

{{< 
    figure src="/fedora/posts/forgejo/feat/img/redesign-forgejo-later.png" title="Refined Design"
    caption="The early design was later refined to make it look nicer, and the placement of elements was also reworked, following what users are comfortable with. This may look fine, but this was hardcoded to show and get a green light to proceed with this design. It is still in the early phase. Also, the SHA box looks a bit thicker and looks odd compared to the other elements in the commit list."
    alt="current forgejo commit design (as of Jun, 2026)"
>}}

> **Note**: The entire backend was replaced by the time I have made this early design, previously the commit list was being rendered inside the table, however I have rewrite the code with for loops, also it kinda made it easier for me group the commits by date.

### Finalized Desktop Layout
{{< 
    figure src="/fedora/posts/forgejo/feat/img/final-forgejo-desktop.png" title="Current Deskop Layout of Forgejo Commit Page"
    alt="current forgejo commit design (as of Jun, 2026)"
>}}
This was the **final design that got approved**. Since the refined design already looked good, I made sure to stick with the base SHA box theme and proceeded with designing the final version. I finished almost everything in the design but needed help with pixel-perfect element placement, design refinement, and other assistance in finalizing the design. [0ko](https://codeberg.org/0ko) was incredibly helpful in finalizing the design and supporting me through this process. [0ko](https://codeberg.org/0ko) co-authored the design and reviewed my changes.

Since I'm relatively new to the Forgejo's codebase, I was able to come up with the initial design and lay the foundation, but I clearly needed help with the final changes of the revamp. [0ko](https://codeberg.org/0ko) made sure to assist as much as with this revamp.

### Mobile Layout Ideas

{{< 
    figure src="/fedora/posts/forgejo/feat/img/mock-mobile-redesign.png" 
    width="60%"
    alt="mock design the new mobile layout"
>}}
{{< 
    figure src="/fedora/posts/forgejo/feat/img/mobile-redesign-forgejo-early.png" title="Early Design"
    caption="The mobile layout has been fully rewritten from scratch since the existing code was already broken for the previous design. I used [tldraw](https://www.tldraw.com/) to create a mock design for the new mobile layout, shared it in the PR comments, and received feedback. Then, I started working on a better design that everyone could agree on. Since there was a good amount of engagement on the page, with many people expressing interest in this new design, I received numerous comments and incorporated that feedback to create this final design."
    width="60%"
    alt="current forgejo commit design (as of Jun, 2026)"
>}}

{{< 
    figure src="/fedora/posts/forgejo/feat/img/mobile-redesign-forgejo-later.png" title="Refined Design"
    caption="After some trial and error, we got to this design, and I received help from [0ko](https://codeberg.org/0ko) as well. I almost finished the visual structure of the design, and we needed to make final changes such as SHA box placement, button and ellipsis button placement, and adding a border to highlight them. It took some time to come up with this design, since the mobile layout was completely designed and communicated asynchronously."
    width="60%"
    alt="current forgejo commit design (as of Jun, 2026)"
>}}

### Finalized Mobile Layout
{{< 
    figure src="/fedora/posts/forgejo/feat/img/final-forgejo-mobile.png"
    title="Current Deskop Layout of Forgejo Commit Page"
    alt="current forgejo commit design (as of Jun, 2026)" 
>}}
After some time, we finally settled on this design, and it clearly satisfied all the requirements. We're able to show all the essential information on the mobile layout without having to hide it to save space. [0ko](https://codeberg.org/0ko) was really helpful in perfectly placing the elements and helping me finalize and make the final changes in the mobile layout redesign. We also got suggestions and feedback from the community, who engaged with us and helped complete this layout.

# Bugs Fixed/Found While Working On This Revamp
1. [fix(ui): resolved 500 error upon clicking 'Clear milestone' button when there's no milestones available in Issue page #8266](https://codeberg.org/forgejo/forgejo/pulls/8266)

I found this while I was writing e2e tests using Playwright. It popped out of nowhere, and it clearly doesn't make sense. However, it was a not critical bug so I thought that while I am working on this revamp, I might as well fix this bug.

2.  [bug: DateTime tippy tooltip is being rendered in browser's default language #9143 ](https://codeberg.org/forgejo/forgejo/issues/9143)

This basically renders the tooltip contents in English only, even though the language was set to something different. So I raised this PR [fix: uses lang attribute to set the right language for tippy tooltip #10065](https://codeberg.org/forgejo/forgejo/pulls/10065) and fixed the issue, but I was stuck on writing the tests for it. Since I was already working on the commit revamp, I thought I should focus on one thing at a time. This is WIP as of now, and I was hoping to get my hands on it soon.

# Important Notes

1. For now, this new layout will only appear on the pull request page. In the future, it will be adopted across all of Forgejo.

2. This new layout is scheduled to release in v16.0.0

# What I've Learned
I would say I've gotten comfortable writing code in Go, reading the Go codebase, and navigating it easily to find the relevant files. Additionally, I've gotten comfortable communicating, reaching out for help without holding back, and continuing to learn.

I also got great help from my mentor, [Tomáš Hrčka](https://accounts.fedoraproject.org/user/humaton/), who taught me how to understand and write Playwright test code for Forgejo. His help was very important in finishing the new commit list design. Tomáš has been guiding me the right way since I started working on this revamp. His teaching and guidance matched how I learn and write code, which helped me learn, and find the right resources.

# Conclusion
This was a great learning experience, both technically and non-technically. I not only got the opportunity to learn Go and Playwright, but I also got the chance to talk with and learn from the Forgejo community. I am now part of the Forgejo team as well, and I represent both the Fedora Project and Forgejo in my open source journey.