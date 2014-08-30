---
layout: post
title:  Towards a prettier git status
categories: 
tags:
---

Using a version control system is almost always a good idea, and at [Semantics3](https://semantics3.com), `git` has proven to a life-saver many times over. Given our service oriented  architecture (coming soon to a browser near you), and our per-seat pricing on  GitHub, our trigger-happy developers have ballooned our repository count. With  collaboration at an all-time high and each dev handling multiple repos, it becomes difficult to keep a handle on the current status of our repositories.

Good ol' `ls` only gives limited information and is not particularly git-aware.

![ls](/assets/images/git-multi-status/ls.gif)

So, enter our handy script [`git-multi-status`](https://gist.github.com/ramananbalakrishnan/be74b96ab1700114493a). The small script searches through folders, runs a few git commands to update your repositories and reports on their status.

![gms](/assets/images/git-multi-status/gms.gif)
 
It is of-course a very simple script which looks for `.git` folders and afew particular keywords that turn up when running `git status`. But this simplicity is part of what we like in its operation - the labels `Untracked`, `Modified`, `Staged`, `Unpushed`, `Unmerged` and `Up-to-date` provide just enough information to be processed at a glance.

Some notes:

 - The script is based on a [gist by jcordasc](https://gist.github.com/jcordasc/389478).
 - BSD / OS X users might experience difficulties because the `find` command in  this script was written for use with GNU find

 > originally appeared in the [Engineering@Semantics3](https://engineering.semantics3.com/2016/05/22/ssh-local-port-forwarding/) blog
