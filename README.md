# Stéphane Klein's patches for `etalab-ia/albert-conversation` project

This repository contains my patches for <https://github.com/etalab-ia/albert-conversation> project.

For example [`devkit.patch`](./devkit.patch) which allows me to use [Mise](https://mise.jdx.dev/).

I hope some of these patches will be accepted upstream.

These patchs are managed by [Stacked Git](https://stacked-git.github.io/).

The [gitleaks](https://github.com/gitleaks/gitleaks) tool is used to prevent accidental commits of secrets.

This project is a work in progress and is not yet ready for use.

## Getting started

```sh
$ mise install
```

Check that [gitleaks](https://github.com/gitleaks/gitleaks) works correctly:

```
$ gitleaks git -v --no-banner
5:28PM INF 4 commits scanned.
5:28PM INF scanned ~6012 bytes (6.01 KB) in 114ms
5:28PM INF no leaks found
```

Enable *gitleaks* verification before each commit:

```
$ git config core.hooksPath git-hooks
```

## How to apply patches?

Install <https://stacked-git.github.io/>.

Here's how to install Stacked Git on Fedora:

```sh
$ sudo dnf install stgit
$ stg --version
Stacked Git 2.5.1
Copyright (C) 2005-2024 StGit authors
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
SPDX-License-Identifier: GPL-2.0-only
git version 2.49.0
```

Clone [albert-conversation](https://github.com/etalab-ia/albert-conversation) repository in `./upstream/`:

```
$ git clone git@github.com:etalab-ia/albert-conversation.git upstream
$ cd upstream
```

Apply patch with *stacked-git*:

```
$ stg import ../devkit.patch
```

Then, follow the instructions in `./upstream/README.sklein.md`.

## Tips

### How can I remove the patch?

```sh
$ cd upstream
$ stg pop
```

### How to create a new patch for albert-conversation?

```sh
$ cd upstream
$ stg import ../devkit-patch.patch
$ stg new mysuper-feature.patch -m "My super private feature"
> mysuper-feature.patch (new)

$ stg series
+ devkit-patch.patch
> mysuper-feature.patch

$ git log --oneline
6836ed4 (HEAD -> main) My super private feature
2f56dae Add devkit
d0ab6c8 (origin/main, origin/HEAD) First import
```

I make some changes in the project:

```sh
$ echo "Add to file1" >> file1.js
$ cat file1.js
$ echo "Add to README" >> README.md
$ cat README.md
$ echo "New file4" >> file4.js
```

I check the changes with *stg*:

```sh
$ stg status
 M README.md
 M file1.js
?? file4.js
```

I add the modified files in the patch and perform a refresh:

```sh
$ stg add file4.js
$ stg add README.md file1.js
$ stg refresh
```

I export the patches to the parent Git repository ([`albert-conversation-sklein-patchs`](https://github.com/stephane-klein/albert-conversation-sklein-patchs)):

```sh
$ stg export -d ../
```

I check which patches have been modified:

```sh
$ cd ..
$ git status -s
 M series
?? mysuper-feature.patch
$ cat series
# This series applies on Git commit d0ab6c8d4c56f71b63b13fc6aa3a965eaf1a6ebd
devkit-patch.patch
mysuper-feature.patch
```

### Gitleaks

Run gitleaks on local files that have not yet been committed:

```
$ gitleaks dir -v --no-banner
5:41PM INF scanned ~15230462 bytes (15.23 MB) in 472ms
5:41PM INF no leaks found
```

If *gitleaks* returns something like this:

```
Finding:     ... slot.\");for(var a=t._keys,n=a.length;r<n;){var i=Object.c...
Secret:      n=a.length
RuleID:      generic-api-key
Entropy:     3.121928
File:        upstream/static/dsfr.nomodule.min.js.map
Line:        1
Fingerprint: upstream/static/dsfr.nomodule.min.js.map:generic-api-key:1
```

and if this secret detection is a false positive, then it is possible to ignore this detection by adding its `Fingerprint` in `.gitleaksignore`.

More information about *gitleaks*: https://notes.sklein.xyz/2025-05-07_2353/

