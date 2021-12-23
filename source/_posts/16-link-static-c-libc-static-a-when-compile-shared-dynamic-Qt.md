---
title: 16.How to link static c++  'libc++_static.a' when compile shared dynamic Qt
author: cyclezone
img: /medias/banner/3.jpeg
top: false
cover: false
coverImg: /medias/banner/1.jpeg
password: 8d969eef6
toc: false
mathjax: false
summary: How to link static c++  'libc++_static.a' when compile shared dynamic Qt
categories: cycle zone
tags:
  - cycle
date: 2021-12-19 21:07:13
---

# 16. link static c++  'libc++_static.a' when compile shared dynamic Qt

when compile dynamic Qt library,we use the configure item -shared;
then qmake generate *.pro file into the makefile whith the corresponding configuration;
in the end ,we get the *.so file which link the dynamic c++  file "libc++_shared.so";

```
  PS D:\android\program\adt\ndk_r16b\toolchains\arm-linux-androideabi-4.9\prebuilt\windows-x86_64\arm-linux-androideabi\bin> 
  .\readelf.exe -d E:\qt\libQt5Core.so

  Dynamic section at offset 0x609670 contains 31 entries:
    Tag        Type                         Name/Value
  0x0000000000000001 (NEEDED)             Shared library: [libz.so]
  0x0000000000000001 (NEEDED)             Shared library: [libc++_shared.so]
  0x0000000000000001 (NEEDED)             Shared library: [liblog.so]
  0x0000000000000001 (NEEDED)             Shared library: [libm.so]
  0x0000000000000001 (NEEDED)             Shared library: [libdl.so]
  0x0000000000000001 (NEEDED)             Shared library: [libc.so]
  0x000000000000000e (SONAME)             Library soname: [libQt5Core.so]
```

## How to link the static C++?

1. locate the makefile which match with 'corelib.pro';
2. open the makefile,find the TAG "LIBS",from this tag,we know the libraries which libQt*.so depend on ,inculde libc++_shared.so ;
  
	```
	LIBS          = $(SUBLIBS)  D:/android/program/adt/ndk-r20b/platforms/android-21/arch-arm/usr/lib/libz.so 
	E:/Qt/buildr20b/qtbase/lib/libqtpcre2.a 
	-LD:\android\program\adt\ndk-r20b/sources/cxx-stl/llvm-libc++/libs/armeabi-v7a D:\android\program\adt\ndk-r20b/sources/cxx-stl/llvm-libc++/libs/armeabi-v7a/libc++.so.21 -llog -lz -lm -ldl -lc 
	-LD:\android\program\adt\ndk-r20b/sources/cxx-stl/llvm-libc++/libs/armeabi-v7a D:\android\program\adt\ndk-r20b/sources/cxx-stl/llvm-libc++/libs/armeabi-v7a/libc++.so.21 -llog -lz -lm -ldl -lc

	```

3. in source code folder ../qtbase/ , search key words '-llog -lz -lm -ldl -lc' ;
	we find the file 'android-base-tail.conf'
    ```
      QMAKE_LIBS_PRIVATE      = $$ANDROID_CXX_STL_LIBS -llog -lz -lm -ldl -lc
    ```       
4. then search key words 'ANDROID_CXX_STL_LIBS'
   we find the file 'qmake.conf'

	 ```
    exists($$ANDROID_SOURCES_CXX_STL_LIBDIR/libc++.so): \
      ANDROID_CXX_STL_LIBS = -lc++
    else: \
      ANDROID_CXX_STL_LIBS = $$ANDROID_SOURCES_CXX_STL_LIBDIR/libc++.so.$$replace(ANDROID_PLATFORM, "android-", "")
   ```

5. according to the tag 'LIBS' information list in step 2,
  guess 'exists($$ANDROID_SOURCES_CXX_STL_LIBDIR/libc++.so)' return false; so modify file 'qmake.conf':

    ```
        exists($$ANDROID_SOURCES_CXX_STL_LIBDIR/libc++.so): \
          ANDROID_CXX_STL_LIBS = -lc++
        else: \
          ANDROID_CXX_STL_LIBS = $$ANDROID_SOURCES_CXX_STL_LIBDIR/libc++.a.$$replace(ANDROID_PLATFORM, "android-", "")
    ```
6. rebuild Qt and check the file libQt*.so
  
    ```    
    PS D:\android\program\adt\ndk_r16b\toolchains\arm-linux-androideabi-4.9\prebuilt\windows-x86_64\arm-linux-androideabi\bin> 
    .\readelf.exe -d E:\Qt\libQt5Core.so

    Dynamic section at offset 0x440508 contains 32 entries:
    Tag        Type                         Name/Value
    0x00000003 (PLTGOT)                     0x441e74
    0x00000002 (PLTRELSZ)                   33536 (bytes)
    0x00000017 (JMPREL)                     0xa36a0
    0x00000014 (PLTREL)                     REL
    0x00000011 (REL)                        0x94eb8
    0x00000012 (RELSZ)                      59368 (bytes)
    0x00000013 (RELENT)                     8 (bytes)
    0x6ffffffa (RELCOUNT)                   2995
    0x00000006 (SYMTAB)                     0x1f0
    0x0000000b (SYMENT)                     16 (bytes)
    0x00000005 (STRTAB)                     0x20890
    0x0000000a (STRSZ)                      337204 (bytes)
    0x6ffffef5 (GNU_HASH)                   0x72dc4
    0x00000004 (HASH)                       0x80b74
    0x00000001 (NEEDED)                     Shared library: [libz.so]
    0x00000001 (NEEDED)                     Shared library: [liblog.so]
    0x00000001 (NEEDED)                     Shared library: [libm.so]
    0x00000001 (NEEDED)                     Shared library: [libdl.so]
    0x00000001 (NEEDED)                     Shared library: [libc.so]
    0x0000000e (SONAME)                     Library soname: [libQt5Core.so]
    0x0000001a (FINI_ARRAY)                 0x437228
    ```
      
        