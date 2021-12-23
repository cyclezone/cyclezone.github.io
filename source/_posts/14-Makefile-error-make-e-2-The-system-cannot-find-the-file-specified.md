---
title: '14. Makefile error make (e=2): The system cannot find the file specified'
author: cyclezone
img: /medias/banner/3.jpeg
top: false
cover: false
coverImg: /medias/banner/1.jpeg
password: 8d969eef6
toc: false
mathjax: false
summary: 'Makefile error make (e=2): The system cannot find the file specified'
categories: cycle zone
tags:
  - cycle
date: 2021-12-19 20:58:12
---


# 14. Makefile error make (e=2): The system cannot find the file specified


1. when compile Qt for android in Windows Environment, crash the following case:
   
  ```    
    cannot find E:\Qt\qtbase\src\android\jar\QtAndroid.jar
    jar cf QtAndroid.jar -C .classes .
    process_begin: CreateProcess(NULL, jar cf QtAndroid.jar -C .classes ., ...) failed.
    make (e=2): The system cannot find the file specified
   ```
2. almost certainly complaining that Windows cannot find 'jar'.
   
   This is almost certainly because the value of %PATH% (or whatever) is different when make spawns a shell/console then when you have it open manually.
   
   Compare the values to confirm that. Then either use the full path to 'jar' in the makefile recipe or ensure that the value of PATH is set correctly for make's usage

3. in console , print 'where java',then show the full path of java.exe;

   but when print 'where jar',cannot show the full path of 'jar.exe';
   in path %JAVA_HOME%\bin, we can find jar.exe  essentially, but now we cannot;

   when we check the JAVA folder in other PC ,we find that file 'jar.exe';
   then copy the qt source code, compile java code,
   run 'jar cf QtAndroid.jar -C .classes .';
   that work OK;

   so  the java environment maybe has been broken sometimes when we install other program;

   copy the jar.exe into the target PC or reinstall JDK in the target PC,
   then check,OK,we solve this problem;

4. besides this,when search this topic key words,we find the similar question;
   almost certainly complaining that Windows cannot find some target process,

   what we should do is locate the target process path,make the compiler know the path,
   find the target process in the path;

5. fro more information, please visit 
   * https://stackoverflow.com/questions/33674973/makefile-error-make-e-2-the-system-cannot-find-the-file-specified  
   or
   * https://stackoverflow.com/questions/7291442/error-cannot-run-program-jar-createprocess-error-2-the-system-cannot-find-t
