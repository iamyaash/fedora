+++
title = 'Forgejo UI Changes for Fedora Project'
date = 2025-05-07T10:31:57+05:30
draft = false
summary = "I’ll walk through the frontend contributions I made to the Forgejo UI as part of the Fedora Project."
tags = ["forgejo", "wrap"]
+++

# **Introduction**
As part of Fedora’s infrastructure improvements, we’re bringing source code, issue tracking, and project hosting together onto one platform: **Forgejo**, deployed on the Fedora OpenShift Cluster. This effort **aims to replace legacy systems like Bugzilla and sun-setting Pagure** (_both `src.fedoraproject.org` and `pagure.io`_). In doing so, we’re also providing a clear migration path for Fedora Project repositories currently hosted on external platforms, ensuring a smoother and more centralized development experience moving forward.

***[Read Git Forgejo Initiative 2025](https://fedoraproject.org/wiki/Initiatives/Git_Forge_Initiative_2025)**

I volunteered to take on the responsibility of working on the frontend development for the project. Throughout this process, I’ve been fortunate to have the guidance and mentorship of [Tomáš Hrčka](https://fedoraproject.org/wiki/User:Humaton), who has been helping me navigate the challenges and ensure we’re moving in the right direction.

> **Note**: Throughout this project I will be using both the documentations from https://docs.gitea.com/ and https://forgejo.org/docs.

# 1. **Theme Color Appearance Modifications**
My first assignment was customizing the color appearance of the theme, which I felt hard to navigate at first. Because, there is no mention of theme color modifications in Forgejo documentation. So, I visited [docs.gitea.com](https://docs.gitea.com) and it clearly explains everything.

```
The source files can be found in the following directories:

CSS styles: web_src/css/
JavaScript files: web_src/js/
Vue components: web_src/js/components/
Go HTML templates: templates/
```

Navigate to the `web_src/css/themes/*` directory, where you'll find the `CSS` styling files for Forgejo's theme. To ensure consistency with Fedora's branding, make sure to refer to the [**official color palette for Fedora Project**](https://docs.fedoraproject.org/en-US/project/brand/#_colors), which follows the Fedora Project's color scheme.

With the color palette in hand, we can now proceed with updating the theme colors, making changes one step at a time. Use your browser’s developer tools to check whether your modifications are applied correctly or a testing environment.

I’ve created two new files for the dark (`theme-fedora-dark.css`) and light (`theme-fedora-light.css`) themes. The main focus will be on updating the `--color-primary` and `--color-secondary` variables. It's important to select the appropriate shades carefully from the color range to match Fedora’s theme standards.

> Here’s the branch with the theme changes: [https://codeberg.org/dovah-kiin/forgejo-fedora](https://codeberg.org/dovah-kiin/forgejo-fedora)

**References (_used to pick colors by visually comparing them_)**:
- https://colorpicker.fr/
- https://imagecolorpicker.com/
- https://coolors.co/
- https://docs.fedoraproject.org/en-US/project/brand/

# 2. **Logo Change**

**The Process**:

The process of customizing the logo is pretty much simple and straightforward. The logos are stored inside `assets/` directory. In order to change the logo,

1. Replace the `logo.svg` with the custom logo (do the same for changing the `favicon.svg` as well).
2. Run the command in root directory of forgejo/*:
    ```sh
    make generate-images
    ```
This process will generate the assets for logo inside the `assets/*` directory. Keep in mind that changing the logo just by renaming it won’t work!

**The Bug: Conflicts with Assets Generation**

While generating the assets, I ran into an issue, which I reported previously here:
- [Issue $7232 in Codeberg](https://codeberg.org/forgejo/forgejo/issues/7232)

But, I discovered that installing a package called `canvas` resolved the issue and allowed me to generate the assets successfully.

```sh
npm install canvas
```

Later, the problem was addressed by replacing `canvas` with `sharp`. Although I was asked to investigate the root cause of the issue, I didn’t have enough time. However, the issue was eventually fixed within a few days.

It was definitely a worthwhile experience, as it gave me the opportunity to debug the issue and find a solution on my own.

# 3. **Font Change**
This is straightforward, that changing a few elements in `/web_src/css/base.css` would do it. Overall, I changed the only font for English to “Inter” and also root font as well in `base.css`.
```diff
-  --fonts-proportional: -apple-system, "Segoe UI", system-ui, Roboto, "Helvetica Neue", Arial;
+  --fonts-proportional: "Inter", sans-serif !important; 

:root * {
-  --fonts-regular: var(--fonts-override, var(--fonts-proportional)), "Noto Sans", "Liberation Sans", sans-serif, var(--fonts-emoji);
+  --fonts-regular: var(--fonts-override, var(--fonts-proportional)), "Inter", sans-serif, var(--fonts-emoji);
}
```

**The Research Part** :

[**Pull Request: font-customizations #13**](https://codeberg.org/fedora/forgejo/pulls/13)

There were some questions raised about whether Forgejo uses a third-party CDN to load fonts or if fonts are bundled during the build process. If it relied on external fonts, we needed to ensure the font would be served from our own infrastructure.

To investigate, I started digging into the codebase, looking for any font-related functions or references to external CDNs. However, I couldn’t find a clear answer, so I reached out to the [Forgejo team via their Matrix channel](https://matrix.to/#/#forgejo-development:matrix.org).

They clarified that Forgejo neither uses a CDN for fonts nor bundles them during the build process. Instead, fonts are referenced via general font-family declarations. If the specified font exists on the system, it will be used; otherwise, the browser falls back to default fonts available on the user’s system.

For example,
```diff
:root * {
-  --fonts-regular: var(--fonts-override, var(--fonts-proportional)), "Noto Sans", "Liberation Sans", sans-serif, var(--fonts-emoji);
+  --fonts-regular: var(--fonts-override, var(--fonts-proportional)), "Inter", sans-serif, var(--fonts-emoji);
}
```
1. `Inter` is now the primary font, it will be used if it’s installed on the user's system
2. If not, the browser will fall back to any available `sans-serif` font.

### 4. **Border Color Fix for Light Theme** (_minor fix_)
[**Pull Request: minor-fix: added navbar border color in light-theme #17**](https://codeberg.org/fedora/forgejo/pulls/17)

I added a border color to the vertical navbar in the light theme.
This detail was missed during earlier changes, so I’ve updated it now to ensure visual consistency when switching themes.

# 5. **Building Process**

I primarily used Docker to build and test Forgejo. The build process initially took around 40–45 minutes on a 3 MB/s connection, which felt _unusually long_. The final image size was about 200 MB, which seemed suspicious given the amount of data transferred.

### Building the Forgejo Docker Image:

To build Forgejo’s Docker image, you need to have both Docker and the Docker Buildx plugin installed. You can build the image using either of the following methods:

**Using Buildx (recommended)**:
```sh
docker buildx build --output=type=docker --tag forgejo:mybuild .
```
**Using standard Docker build**:
```sh
docker build --tags forgejo:mybuild .
```
#### Problem
At one point, I started encountering build errors that were difficult to troubleshoot. I tried various approaches, tweaking environment variables, hardcoding dependencies, and modifying build settings, but nothing worked. Since each build took around 40 minutes, debugging was extremely time-consuming and ended up consuming an entire day just to test small changes.

#### Solution
The issue was caused by how Forgejo’s `Dockerfile` handles Go module fetching. By default, it sets the following environment variable:
```sh
ENV GOPROXY=${GOPROXY:-direct}
```
The `direct` setting fetches Go modules directly from the source, which can lead to throttling of your system’s public IP. The Go proxy limits requests to 10 per second, and once that limit is reached, your IP may be temporarily blocked or slowed down, causing the build to fail or hang.

**Error Log**:
```sh
Dockerfile:39
--------------------
  37 |     RUN make frontend
  38 |     RUN go build contrib/environment-to-ini/environment-to-ini.go && xx-verify environment-to-ini
  39 | >>> RUN LDFLAGS="-buildid=" make RELEASE_VERSION=$RELEASE_VERSION GOFLAGS="-trimpath" go-check generate-backend static-executable && xx-verify gitea
  40 |     
  41 |     # Copy local files
--------------------
ERROR: failed to solve: process "/bin/sh -c LDFLAGS=\"-buildid=\" make RELEASE_VERSION=$RELEASE_VERSION GOFLAGS=\"-trimpath\" go-check generate-backend static-executable && xx-verify gitea" did not complete successfully: exit code: 2
i was trying build forgejo docker image from source, but the build failed in the middle with this errors
```
```sh
79.09 go: downloading code.forgejo.org/forgejo/go-xsd-duration v0.0.0-20220703122237-02e73435a078
514.4 cmd/dump.go:23:2: unrecognized import path "code.forgejo.org/go-chi/session": parsing code.forgejo.org/go-chi/session: XML syntax error on line 1: invalid UTF-8
514.4 cmd/forgejo/f3.go:20:2: unrecognized import path "code.forgejo.org/f3/gof3/v3": parsing code.forgejo.org/f3/gof3/v3: XML syntax error on line 1: invalid UTF-8
514.4 cmd/forgejo/f3.go:21:2: unrecognized import path "code.forgejo.org/f3/gof3/v3": parsing code.forgejo.org/f3/gof3/v3: XML syntax error on line 1: invalid UTF-8
514.4 cmd/forgejo/f3.go:22:2: unrecognized import path "code.forgejo.org/f3/gof3/v3": parsing code.forgejo.org/f3/gof3/v3: XML syntax error on line 1: invalid UTF-8
```

To fix this, you can use the official Go proxy by setting in `Dockerfile`:
```sh
GOPROXY=https://proxy.golang.org,direct
```
This configuration first tries the Go proxy (which is faster and more reliable) and falls back to direct fetching only if needed.

You can apply this setting during the Docker build using a build time argument:
```sh
docker buildx build \
  --build-arg GOPROXY=https://proxy.golang.org,direct \
  --output type=docker \
  --tag forgejo:test-build .
```
> **Note**: _By using this build time argument, the build process ends within 6 minutes._
# 6. **Interactive Testing**

Initially, I wasn’t aware that Forgejo could be run locally for testing. For a while, I was relying on browser dev tools and Docker builds to preview changes. Eventually, I discovered the proper way to run a local instance using:

```bash
TAGS='sqlite sqlite_unlock_notify' make watch
```

This runs Forgejo locally, but you’ll need to manually set up a database for it to work properly. Since it was my first time handling databases, I documented the full process in a separate guide.

***[**Read the full testing environment & database setup guide here**](https://iamyaash.github.io/fedora/posts/forgejo/ui/forgejo-interactive-testing/)**

# 7. **Commit Table UI Overhaul**

Forgejo uses an older commit table layout that may be familiar to developers but can be confusing for general users, especially those coming from platforms like GitHub or GitLab. Interestingly, Pagure also uses this outdated design. In these older layouts, the only way to see when a commit was made is by hovering over the date column to reveal a tooltip showing the full date and time.

The goal of the new design is to improve clarity by grouping commits by date directly in the interface, removing the need to hover just to understand when changes occurred. Since we're transitioning from Pagure to Forgejo, this is a great opportunity to introduce a more modern UI and enhance the user experience.

Updating the commit view to group entries by date will make the interface cleaner and more user-friendly, particularly for Fedora’s use cases.

***[**Read about my progress in the detailed post here.**](https://iamyaash.github.io/fedora/posts/forgejo/ui/commit-ui-revamp/)**

***[**Here’s the branch with the new commit table layout**](https://codeberg.org/dovah-kiin/forgejo-official/src/branch/commit-list-layout-revamp)**

# **Conclusion**
I’m still early in the project and have a long way to go, but this has been a great start to my open-source journey. I’m truly grateful for the opportunity and making the most of it.

**What I’ve Learned and Improved On**:
1. I've become more comfortable communicating in public channels, even though I’m not naturally good at social interactions and communication. It’s something I’m actively working on, and while I’m still learning, I’ve been volunteering more and getting better at contributing effectively.

2. I’ve started learning **Golang, Tailwind CSS, and Golang templates** for frontend rendering. I’m using [Exercism](https://exercism.org/) to help with the fundamentals, and it’s been a great resource. Though I’ve broken a few things while experimenting, I’m gaining a lot of knowledge.

3. The feeling of contributing to something meaningful is incredible, especially knowing that my work will be used by many people. It’s been truly rewarding.

