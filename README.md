# PEPR IA Foundry website

You will need `hugo` installed, see instructions here: https://gohugo.io/installation/

How to build:
```
git submodule update --init --recursive
hugo
```

In order to serve locally the website, run:
```
hugo serve --watch
```
and open the website at the indicated localhost port.

## Content

The important files to change are in `content`. The theme is beautifulhugo: https://github.com/halogenica/beautifulhugo.

The top-level menu can be changed from `hugo.toml`.
Pages can be added manually in `content/page` or with `hugo new content content/posts/my-first-post.md`.
