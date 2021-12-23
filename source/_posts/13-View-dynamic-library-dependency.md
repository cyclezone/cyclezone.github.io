---
title: 13. View dynamic library dependency
author: cyclezone
img: /medias/banner/3.jpeg
top: false
cover: false
coverImg: /medias/banner/1.jpeg
password: 8d969eef6
toc: false
mathjax: false
summary: View dynamic library dependency
categories: cycle zone
tags:
  - cycle
date: 2021-12-19 20:56:29
---


# 13. View dynamic library dependency

   * for Windows,  use dependency-walker
     
   * for linux,  use ldd command 
		```
		ldd libjpeg.so
		ldd: warning: you do not have execution permission for `./libjpeg.so'
		linux-vdso.so.1 =>  (0x00007ffea15a2000)
		libc.so.6 => /lib64/libc.so.6 (0x00007f8949b12000)
		/lib64/ld-linux-x86-64.so.2 (0x00007f894a138000)
		```
   * for android , use readelf in ndk,

		```
			PS D:\android\program\adt\ndk_r16b\toolchains\arm-linux-androideabi-4.9\prebuilt\windows-x86_64\arm-linux-androideabi\bin> 
			.\readelf.exe -d D:\android\samples\app\libs\armeabi-v7a\libQt5Core.so

			Dynamic section at offset 0x72f0a4 contains 33 entries:
			Tag        Type                         Name/Value
			0x00000003 (PLTGOT)                     0x730784
			0x00000002 (PLTRELSZ)                   28896 (bytes)
			0x00000017 (JMPREL)                     0x73c8c
			0x00000014 (PLTREL)                     REL
			0x00000011 (REL)                        0x6aba4
			0x00000012 (RELSZ)                      37096 (bytes)
			0x00000013 (RELENT)                     8 (bytes)
			0x6ffffffa (RELCOUNT)                   1956
			0x00000006 (SYMTAB)                     0x1f0
			0x0000000b (SYMENT)                     16 (bytes)
			0x00000005 (STRTAB)                     0x18b50
			0x0000000a (STRSZ)                      236876 (bytes)
			0x6ffffef5 (GNU_HASH)                   0x5289c
			0x00000004 (HASH)                       0x5d790
			0x00000001 (NEEDED)                     Shared library: [libz.so]
			0x00000001 (NEEDED)                     Shared library: [libc++_shared.so]
			0x00000001 (NEEDED)                     Shared library: [liblog.so]
			0x00000001 (NEEDED)                     Shared library: [libm.so]
			0x00000001 (NEEDED)                     Shared library: [libdl.so]
			0x00000001 (NEEDED)                     Shared library: [libc.so]
			0x0000000e (SONAME)                     Library soname: [libQt5Core.so]
			0x0000001a (FINI_ARRAY)                 0x730088
			0x0000001c (FINI_ARRAYSZ)               16 (bytes)
			0x00000019 (INIT_ARRAY)                 0x730098
			0x0000001b (INIT_ARRAYSZ)               12 (bytes)
			0x0000001e (FLAGS)                      BIND_NOW
			0x6ffffffb (FLAGS_1)                    Flags: NOW
			0x6ffffff0 (VERSYM)                     0x679fc
			0x6ffffffc (VERDEF)                     0x6ab28
			0x6ffffffd (VERDEFNUM)                  1
			0x6ffffffe (VERNEED)                    0x6ab44
			0x6fffffff (VERNEEDNUM)                 3
			0x00000000 (NULL)                       0x0
		```
