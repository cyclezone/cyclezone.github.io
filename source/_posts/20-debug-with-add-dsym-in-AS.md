---
title: 20. How to debug with add-dsym in AS
author: cyclezone
img: /medias/banner/3.jpeg
top: false
cover: false
coverImg: /medias/banner/1.jpeg
password: 8d969eef6
toc: false
mathjax: false
summary: How to debug with add-dsym in AS
categories: cycle zone
tags:
  - cycle
  - add-dsym
date: 2021-12-19 21:15:54
---


# 20. debug with add-dsym in AS

## 20.1 detail version

1. Compile the codes and Debug the app;
2. in Debug Tab,select app Tab,then select LLDB Tab;
   here we cannot attach the debug symbol;
3. in left dock buttons, click the pause button to pause the program,then select LLDB Tab;
4. in LLDB tab,attach the library symbols;just like:
   add-dsym E:/Qt/build/armeabi-v7a/qtbase/plugins/platforms/android/libqtforandroid.so
   if need to attach more than one library,call add-dsym one by one;
5. when we have add the target libraries, we can see the information:

   ``` 
    ‘symbol file 'E:\Qt\build\armeabi-v7a\qtbase\plugins\platforms\android\libqtforandroid.so' 
    has been added to 
    'C:\Users\Administrator\.lldb\module_cache\remote-android\.cache\A3E31994\libqtforandroid.so'’
   ```     
6. in left dock buttons, click the resume button to resume the program;
7. then contine to debug the program; 
8. Step into the attached library;


## 20.2 short version

1. Debug ---> app --->LLDB
2. pause
3. add-dsym 
4. resume
5. debug

  ```
  Executing commands in 'D:\android\Android Studio\bin\lldb\shared\stl_printers\load_script'.
  (lldb) script import sys
  (lldb) script import os
  (lldb) script gala_available = os.environ.get('AS_GALA_PATH') is not None
  (lldb) script exec("if gala_available: sys.path.append(os.environ['AS_GALA_PATH'])")
  (lldb) script exec("if gala_available: import gdb")
  (lldb) script exec("if gala_available: import gdb.printing")
  (lldb) script libstdcxx_printers_available = gala_available and (os.environ.get('AS_LIBSTDCXX_PRINTER_PATH') is not None)
  (lldb) script exec("if libstdcxx_printers_available: sys.path.append(os.environ['AS_LIBSTDCXX_PRINTER_PATH'])")
  (lldb) script exec("if libstdcxx_printers_available: import printers")
  (lldb) script exec("if libstdcxx_printers_available: printers.register_libstdcxx_printers(None)")
  (lldb) script exec("if lldb.debugger.GetCategory('libstdc++-v6').IsValid(): lldb.debugger.GetCategory('gnu-libstdc++').SetEnabled(False)")
  (lldb) add-dsym E:/Qt/build/armeabi-v7a/qtbase/plugins/platforms/android/libqtforandroid.so
  symbol file 'E:\Qt\build\armeabi-v7a\qtbase\plugins\platforms\android\libqtforandroid.so' has been added to 'C:\Users\Administrator\.lldb\module_cache\remote-android\.cache\9BBA21BF\libqtforandroid.so'
  (lldb) add-dsym E:/Qt/build/armeabi-v7a/qtbase/lib/libQt5Gui.so
  symbol file 'E:\Qt\build\armeabi-v7a\qtbase\lib\libQt5Gui.so' has been added to 'C:\Users\Administrator\.lldb\module_cache\remote-android\.cache\8D29BD13\libQt5Gui.so'

  ```