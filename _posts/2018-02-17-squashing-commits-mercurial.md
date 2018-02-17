---
layout: post
title: "Squashing Commits in Mercurial"
tags: [mercurial]
---

I'm new to Mercurial, so this is definitely not something trivial for me. 😅 I'm writing this down here in case others may find it useful, including Future Me (I'm *that* forgetful).

## The problem
I was working on an issue and committed my patch before realizing that I still need to do another thing. So I committed my latest update before I realized that I need to do something else, and I need to merge all of these different commits into one commit since they're related to one particular issue. Moreover, I needed to rewrite my commit message because I didn't follow the convention the first time.

When running `hg log`, this is how my log looks like:

```
changeset:  398108:e6478a92425f
tag:        tip
user:       Galuh <foo@bar.com>
date:       Thu Jan 11 20:06:14 2018 +0700
summary:    Commit message #3

changeset:  398107:e2348a34dc00
user:       Galuh <foo@bar.com>
date:       Thu Jan 11 20:03:22 2018 +0700
summary:    Commit message #2

changeset:  398106:ece3485a435g
user:       Galuh <foo@bar.com>
date:       Thu Jan 11 19:54:14 2018 +0700
summary:    Commit message #1

...

```

There are three different commits that I'd like to merge into one commit, with the final commit message modified.

## The solution
I needed something similar to `git rebase`, which doesn't exist in Mercurial unless you use extensions. 

Upon searching I figured that there are a few ways to do this: by using the [`MqExtension`](https://www.mercurial-scm.org/wiki/MqExtension) extension, using the [`Histedit`](https://www.mercurial-scm.org/wiki/HisteditExtension) extension, and using the [`Rebase Extension`](https://www.mercurial-scm.org/wiki/RebaseExtension). All three extensions are already distributed along with Mercurial releases, so you don't need to install them. However, you still need to enable them before using.

What worked for me the first time was `Histedit`, a history editing plugin for Mercurial, so that's the extension I'm going to use throughout this post. What I found useful is that `Histedit` also works similarly to `git rebase --interactive`, which is nice when you're already used to its `git` counterpart.

### Enabling histedit
To enable the `Histedit` extension (or any extension), open the file `$HOME/.hgrc`. This will open a Mercurial configuration file that is set per user. Mercurial also has [other configuration files](https://www.selenic.com/mercurial/hgrc.5.html), which you should check out if you have different needs.

Find a line that says `[extensions]`. This line is a header for the section that keeps track of which extensions you're using. An example would be something like this:

```
...

[extensions]
rebase =
fsmonitor =
blackbox =
reviewboard = <path to the reviewboard extension>

...
```

Since `Histedit` is already installed along with the Mercurial release--unlike the `reviewboard` extension, for example--we do not need to specify the path to the extension. We can just simply add `histedit = ` and we're good to go.

```
...

[extensions]
histedit =
rebase =
fsmonitor =
blackbox =
reviewboard = <path to the reviewboard extension>

...
```

## Squashing the commits

To start working with `histedit`, run the following command:

```
hg histedit -r-3
```

You can change the number `3` according to your need. If you only need to squash the last two commits, then run `hg histedit -r-2`. In my case, since I wanted to squash the last three commits, I ran `hg histedit -r-3` instead.

You would be greeted with an editor that looks like this:

```

pick ece3485a435g Commit message #1
pick e2348a34dc00 Commit message #2
pick e6478a92425f Commit message #3

# Edit history between ece3485a435g and e6478a92425f
#
# Commands:
#  p, pick = use commit
#  e, edit = use commit, but stop for amending
#  f, fold = use commit, but fold into previous commit
#  r, roll = use commit, but fold into previous commit 
   and drop commit message
#  d, drop = remove commit from history
#  m, mess = edit message without changing commit content
#
0 files updated, 0 files merged, 0 files removed, 0 files 
unresolved

```

Mainly there are two things that we can do here: we can reorder the order of the changesets and/or we can modify the action (which is one of pick, edit, fold, roll, drop, or mess) on each changeset.

To squash all the displayed changesets, we need to change the two most recent changesets to `fold`. This will fold the changesets into least recent commit.

```

pick ece3485a435g Commit message #1
fold e2348a34dc00 Commit message #2
fold e6478a92425f Commit message #3

...

```

Save the file and run `hg commit`. You'll be asked to write a new commit message, so you can edit your commit message here. Then, run `hg push`.

If you want to make sure that everything works as intended, run `hg log` and (hopefully) you're left with one commit instead of three like previously:

```
changeset:  398106:ece3485a435g
user:       Galuh <foo@bar.com>
date:       Thu Jan 11 19:54:14 2018 +0700
summary:    New commit message

...

```

For more information on the `Histedit` extension, click [here](https://www.mercurial-scm.org/wiki/HisteditExtension).