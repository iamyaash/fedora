+++
title = 'Forgejo Theming'
date = 2025-03-03T21:45:39+05:30
draft = false
+++

# Customizing the Logo
The process of customizing the logo is pretty much simple and straightforward. The logos are stored inside `assets/` directory. In order to change the logo,

1. Replace the `logo.svg` with the custom logo (do the same for changing the `favicon.svg` as well).
2. Run the command in root directory of `forgejo/*`:
```sh
make generate-images
```
This process will generate the assets for logo inside the `assets/*` directory. Keep in mind that changing the logo just by renaming it won't work!

> **Note:** If you see an error message stating that `canvas` is not installed in your machine, then install it using `npm install canvas`. 
 
# Customizing the Color Appearance of the Theme
This is not mentioned in the documentation of Forgejo and I found that changing the `CSS` elements manually will help change the colors, and there's no need to use `go templates` to inject custom `CSS` into it.

Head inside the `web_src/css/themes/*` directory and this holds the `CSS` styling elements for the Forgejo's theme. Change the elements manually to suit your needs and then build the docker image.

Renaming the filenames will break the theme, so make sure to update the changes of the theme names inside `modules/settings/ui.go` in the `Themes:` variable. 

> **Note:** Theme names should always starts with `theme-*.css`. Using a custom name will break the build.
