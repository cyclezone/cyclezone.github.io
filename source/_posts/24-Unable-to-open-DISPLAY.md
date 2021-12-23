---
title: 24. Unable to open DISPLAY
author: cyclezone
img: /medias/banner/3.jpeg
top: false
cover: false
coverImg: /medias/banner/1.jpeg
password: 8d969eef6
toc: false
mathjax: false
summary: Unable to open DISPLAY
categories: cycle zone
tags:
  - cycle
date: 2021-12-20 23:14:11
---

# 24. Unable to open DISPLAY
```
java.lang.reflect.InvocationTargetException
Casued by:
java.lang.unsupportedOperationException:Unable to open DISPLAY

```
## solution
* echo $DISPLAY
* export DISPLAY=:1
* xhost +
* run sh run.sh