+++
title = "New Fedora Theme in Forgejo"
date = 2025-03-16T12:40:52+05:30
draft = false
summary = "A new Fedora theme for the Forgejo deployment in Fedora Project"
author = "Yashwanth Rathakrishnan"
[cover]
image = "posts/forgejo/theme/forgejo-fedora-theme.png"
alt = "forgejo-fedoraproject staging environment"
hiddenInList = false
+++

# Forgejo Deployment With New Fedora Theme!
On the 14th March, 2025 the new Fedora theme I was working on was [deployed to the staging environment](https://forgejo.apps.ocp.stg.fedoraproject.org/). It feels amazing to know that my contributions will be used by thousands of people! I started working on this theme in mid-February, and throughout the process, I connected with and learned from many people.

Since I’m still relatively new to working on large, long-term projects, I took my time researching and approached everything step by step. I’ve already shared how I made the theme changes, that it was fairly straightforward, but I had to go through the documentation and sometimes figure things out on my own.

One thing I noticed is that Forgejo’s documentation doesn’t have a guide for changing the theme’s appearance. I had to do a lot of digging and manually modify the CSS elements. My main goal was to ensure the theming was done in a way that makes future maintenance easy, rather than relying on tedious manual fixes. But yeah, the manual way seems more maintainable. 

## How Did I Made The Theme Changes?

1. **Logo & Favicon**
    - Usually, it's not possible to get your hands on the "Fedora" logo, so I had to send an email to **`logo@fedoraproject.org`** to receive the logo.
    - Then, I replaced the Forgejo `logo.svg` & `favicon.svg` in `/assets/*` with Fedora logo. _(i.e, moved the Fedora logo inside, copied two of them and changed the name to `logo.svg` & `favicon.svg`)_.
    - Executing a command in the parent directory of `forgejo/` which would generate the assets to the respective places:
        ```sh
        make generate-images
        ```
    > **Note:** Sometimes it will result in error, by staging `canvas` is not installed, so install `canvas` package just in case the error shows up.

2. **Theme Appearance**
    - Forgejo uses **`tailwind css`** for styling the frontend and it uses **`template`** module from `Go` lang for rendering the UI assets.
    - There isn't any documentation mentioned about changing the theme colors, so I had to dig some things on my own to find a way to change the theme colors with easy-maintainance in mind!
    - I found that the theme related files are stored under **`/web_src/css/themes/*`**.
    - Then I made some research on the Fedora's brand theme and used those colors pick colors for each element, I did mess up bringing the same color on different element, so I took my time and chose each color with precision.
    - At last the I made changes to **Light & Dark theme**, but I feel that only light theme is ready and dark theme could use some refinement. So I'm still working on the dark theme, but the light theme is merged!

3. **Font Changes**
    - This is straightforward, that changing a few elements in `/web_src/css/base.css` would do it.
    - Overall, I changed the only font for English to "Inter" and also `root` font as well in `base.css`.

```diff
-  --fonts-proportional: -apple-system, "Segoe UI", system-ui, Roboto, "Helvetica Neue", Arial;
+  --fonts-proportional: "Inter", sans-serif !important; 

:root * {
-  --fonts-regular: var(--fonts-override, var(--fonts-proportional)), "Noto Sans", "Liberation Sans", sans-serif, var(--fonts-emoji);
+  --fonts-regular: var(--fonts-override, var(--fonts-proportional)), "Inter", sans-serif, var(--fonts-emoji);
} 
```
### Things I Stumbled Upon When Working on Forgejo Themeing

While making the theme changes, 
1. _I encountered a warning when building the image. It was a really simple fix, but my open-source enthusiastic mindset kicked in, and I decided to contribute beyond just my theme work. So, [I raised a PR in **`forgejo/forgejo`** (Forgejo Official)](https://codeberg.org/forgejo/forgejo/pulls/6769) and it was reviewed and merged_ ✅.
2. _I also found an issue when generating custom **`logo.svg`** & **`favicon.svg`** resulted in an error. After some debugging, I realized that installing **`canvas`** package would fix it. [I raised an issue for that for clarification so it's under review](https://codeberg.org/forgejo/forgejo/issues/7232), let's see how it goes!_


Overall, I feel super excited & proud to be working on open-source projects and making meaningful contributions, not just for recognition, but because I genuinely enjoy it 😌.