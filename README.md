# Stéphane Klein's patches for `etalab-ia/albert-conversation` project

This repository contains my patches for <https://github.com/etalab-ia/albert-conversation> project.

For example [`devkit.patch`](./devkit.patch) which allows me to use [Mise](https://mise.jdx.dev/).

I hope some of these patches will be accepted upstream.

## How to apply patches?

Install https://stacked-git.github.io/

```
$ git clone git@github.com:etalab-ia/albert-conversation.git upstream
$ cd upstream
$ stg import ../devkit.patch
```
