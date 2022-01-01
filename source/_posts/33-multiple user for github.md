---
title: 33. Permission to github.io.git denied to another user
author: cyclezone
img: /medias/banner/3.jpeg
top: false
cover: false
coverImg: /medias/banner/1.jpeg
password: 8d969eef6
toc: false
mathjax: false
summary: Permission to username/github.io.git denied to another username
categories: cycle zone
tags:
  - github
  - Permission denied
date: 2021-12-24 23:21:32
---

# 33.Permission to username/github.io.git denied to another username

ERROR: Permission to cyclezone/cyclezone.github.io.git denied to cycle.
fatal: Could not read from remote repository.


```
root@user MINGW64 /e/git/cyclezone.github.io (hexo)
$ git push origin hexo
ERROR: Permission to cyclezone/cyclezone.github.io.git denied to cycle.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

root@user MINGW64 /e/git/cyclezone.github.io (hexo)
$ git log
commit 7f6cb6c3ca90241362045ddc47fdd31995a32eec (HEAD -> hexo)
Author: cycle <ycae#outlook.com>
Date:   Thu Dec 23 22:15:35 2021 +0800

    empty branch

root@user MINGW64 /e/git/cyclezone.github.io (hexo)
$ git push origin hexo
ERROR: Permission to cyclezone/cyclezone.github.io.git denied to cycle.
fatal: Could not read from remote repository.


root@user MINGW64 /e/git/cyclezone.github.io (hexo)
$ git config -l
diff.astextplain.textconv=astextplain
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
http.sslbackend=openssl
http.sslcainfo=D:/Application/program/Git/mingw64/ssl/certs/ca-bundle.crt
core.autocrlf=true
core.fscache=true
core.symlinks=false
core.editor=notepad
pull.rebase=false
credential.helper=manager-core
credential.https://dev.azure.com.usehttppath=true
init.defaultbranch=master
user.name=cycle
user.email=ycae#outlook.com
core.repositoryformatversion=0
core.filemode=false
core.bare=false
core.logallrefupdates=true
core.symlinks=false
core.ignorecase=true
remote.origin.url=git@github.com:cyclezone/cyclezone.github.io.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.main.remote=origin
branch.main.merge=refs/heads/main
user.name=cyclezone
user.email=ycae#outlook.com

root@user MINGW64 /e/git/cyclezone.github.io (hexo)
$ git remote rm origin

root@user MINGW64 /e/git/cyclezone.github.io (hexo)
$ git remote add origin git@cycle.github.com:cyclezone/cyclezone.github.io.git

root@user MINGW64 /e/git/cyclezone.github.io (hexo)
$ git config -l
diff.astextplain.textconv=astextplain
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
http.sslbackend=openssl
http.sslcainfo=D:/Application/program/Git/mingw64/ssl/certs/ca-bundle.crt
core.autocrlf=true
core.fscache=true
core.symlinks=false
core.editor=notepad
pull.rebase=false
credential.helper=manager-core
credential.https://dev.azure.com.usehttppath=true
init.defaultbranch=master
user.name=cycle
user.email=ycae#outlook.com
core.repositoryformatversion=0
core.filemode=false
core.bare=false
core.logallrefupdates=true
core.symlinks=false
core.ignorecase=true
user.name=cyclezone
user.email=ycae#outlook.com
remote.origin.url=git@cycle.github.com:cyclezone/cyclezone.github.io.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*

root@user MINGW64 /e/git/cyclezone.github.io (hexo)
$ git status
On branch hexo
nothing to commit, working tree clean

root@user MINGW64 /e/git/cyclezone.github.io (hexo)
$ git push origin hexo
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Delta compression using up to 8 threads
Compressing objects: 100% (1/1), done.
Writing objects: 100% (2/2), 227 bytes | 227.00 KiB/s, done.
Total 2 (delta 0), reused 0 (delta 0), pack-reused 0
remote:
remote: Create a pull request for 'hexo' on GitHub by visiting:
remote:      https://github.com/cyclezone/cyclezone.github.io/pull/new/hexo
remote:
To cycle.github.com:cyclezone/cyclezone.github.io.git
 * [new branch]      hexo -> hexo

```