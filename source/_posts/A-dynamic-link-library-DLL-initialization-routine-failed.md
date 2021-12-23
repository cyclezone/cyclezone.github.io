---
title: A dynamic link library (DLL) initialization routine failed
author: cyclezone
img: /medias/banner/3.jpeg
top: false
cover: false
coverImg: /medias/banner/1.jpeg
password: 8d969eef6
toc: false
mathjax: false
summary: A dynamic link library (DLL) initialization routine failed
categories: cycle zone
tags:
  - cycle
date: 2021-12-19 20:42:41
---


# 8. A dynamic link library (DLL) initialization routine failed

![initialization routine failed](./img/8_initialization_routine_failed.png)


use IntelliJ IDEA to debug some program, when load some dynamic link library (*.dll),
throw out some error infomation: "A dynamic link library (DLL) initialization routine failed" ;

when check the dll file ,the file exist and the located path has been set in the Computer PATH ENV,
besides this,use the dependency tool(dependency walker), we get the full complete dependent file list in the table view;

the method to solve this situation:

1. cmd --->regedit 
2. locate the target program node;
   eg: HKEY_CURRENT_USER\Software\MapCycle\AppProduct\Developer
3. delete the java node which set the java infomation,
   and node name make consist of the HEX code;



 regedit path
![HKEY_CURRENT_USER_Software_MapCycle](./img/8_initialization_routine_failed_regedit.png)

