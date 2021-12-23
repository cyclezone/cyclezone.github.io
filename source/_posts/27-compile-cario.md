---
title: 27. How to compile cario
author: cyclezone
img: /medias/banner/3.jpeg
top: false
cover: false
coverImg: /medias/banner/1.jpeg
password: 8d969eef6
toc: false
mathjax: false
summary: How to compile cario
categories: cycle zone
tags:
  - cycle
date: 2021-12-21 23:16:53
---

# 27. How to compile cario

## For linux, Steps:

1. get source codes 
	```
	from http://www.cairographics.org/releases/pixman-0.40.0.tar.gz  
	get pixman library;

	from https://cairographics.org/snapshots/cairo-1.17.2.tar.xz
	get cario library;

	or visit url: https://www.cairographics.org/releases/ 
	select the latest version;
	```
2. then copy the source codes into some folder,
   like: /home/src/code/ ;

   in shell terminal,use commands below to decompress the tar file; 
	```
	tar zxvf pixman-0.40.0.tar.gz 
	tar zxvf cairo-1.17.2.tar.xz 
	```
3. compile pixman
  
      open the pixman source code root folder,then configure、 make、install
	```
	cd /home/src/code/pixman-0.40.0

	./configure --prefix=/home/src/code/pixman 

	make

	make install
	```
4. compile cario
      
	4.1 open the cario source code root folder, then configure、 make、install
	```
	cd /home/src/code/cairo-1.17.2

	./configure --prefix=/home/src/code/cario --enable-ps pixman_CFLAGS='-I/home/src/code/pixman/include/pixman-1' pixman_LIBS='-L/home/src/code/pixman/lib -lpixman-1'

	make

	make install
	```
	4.2 how to get the dependent librarys information
	    
		use ' ./configure --help ' get some environment variables，like below;

	```	
	png_CFLAGS  C compiler flags for png, overriding pkg-config
	png_LIBS    linker flags for png, overriding pkg-config 
	gl_CFLAGS   C compiler flags for gl, overriding pkg-config
	gl_LIBS     linker flags for gl, overriding pkg-config
	FREETYPE_CFLAGS
			C compiler flags for FREETYPE, overriding pkg-config
	FREETYPE_LIBS
			linker flags for FREETYPE, overriding pkg-config
	FONTCONFIG_CFLAGS
			C compiler flags for FONTCONFIG, overriding pkg-config
	FONTCONFIG_LIBS
			linker flags for FONTCONFIG, overriding pkg-config
	LIBRSVG_CFLAGS
			C compiler flags for LIBRSVG, overriding pkg-config
	LIBRSVG_LIBS
			linker flags for LIBRSVG, overriding pkg-config
	pixman_CFLAGS
			C compiler flags for pixman, overriding pkg-config
	pixman_LIBS linker flags for pixman, overriding pkg-config
	```
	
	set the environment variables:
	```
	pixman_CFLAGS='-I/home/src/code/pixman/include/pixman-1' 
	pixman_LIBS='-L/home/src/code/pixman/lib -lpixman-1'
	```
5. get cario library
   
	```
	open folder /home/src/code/cario ,get libs
	```
## For Android, Steps:
```
export CPPFLAGS=--sysroot="/opt/android-ndk-r13b/platforms/android-23/arch-arm/ "
export CFLAGS=--sysroot="/opt/android-ndk-r13b/platforms/android-23/arch-arm/ "
export CXXFLAGS=--sysroot="/opt/android-ndk-r13b/platforms/android-23/arch-arm/ "
export LDFLAGS=--sysroot="/opt/android-ndk-r13b/platforms/android-23/arch-arm/ "
export CC="/opt/android-ndk-r13b/toolchains/arm-linux-androideabi-4.9/prebuilt/linux-x86_64/bin/arm-linux-androideabi-gcc "
export CXX="/opt/android-ndk-r13b/toolchains/arm-linux-androideabi-4.9/prebuilt/linux-x86_64/bin/arm-linux-androideabi-g++ "

./configure --host=arm --prefix=/opt/cairo/cairo --with-sysroot="/opt/android-ndk-r13b/platforms/android-23/arch-arm/" --enable-png=yes --disable-xlib --disable-xlib-xrender --enable-directfb=no --enable-ft=yes  --enable-fc=no --disable-win32 --disable-svg --enable-pdf=yes  FREETYPE_CFLAGS='-I/opt/cairo/freetype/include/freetype2' FREETYPE_LIBS='-L/opt/cairo/freetype/lib -lfreetype' png_CFLAGS='-I/opt/cairo/png/include' png_LIBS='-L/opt/cairo/png/lib -lpng16' pixman_CFLAGS='-I/opt/cairo/pix_man/include/pixman-1' pixman_LIBS='-L/opt/cairo/pix_man/lib -lpixman-1' --enable-xcb=no 

```
## For IOS, Steps:

```
#export PKG_CONFIG_PATH=/usr/lib/pkgconfig/
export BUILD_ARCHS=armv7 armv7s arm64 i386 x86_64
#IOS_LINK_FLAG= -arch x86_64
export IOS_BUILD_TOOLS_DIR=/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/
export IOS_SYSROOT_DIR="/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator.sdk"
#export CC="/Applications/Xcode.app/Contents/Developer/usr/bin/gcc "
#export CXX="/Applications/Xcode.app/Contents/Developer/usr/bin/g++ "

#export CC="/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/clang "
#export CXX="/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/clang++ "

./configure --prefix=/opt/simulator_cairo_lib/cairo CC="/Applications/Xcode.app/Contents/Developer/usr/bin/gcc -arch x86_64" --with-sysroot="/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator.sdk" AR="/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/ar" --enable-xlib=no --enable-xlib-xrender=no --enable-ft=no --enable-script=no --enable-ps=no --enable-pdf=no --enable-svg=no --enable-trace=no --enable-interpreter=no --enable-png=no  pixman_CFLAGS='-I/opt/simulator_cairo_lib/pixmap/include/pixman-1' pixman_LIBS='-L/opt/simulator_cairo_lib/pixmap/lib -lpixman-1' png_CFLAGS='-I/opt/simulator_cairo_lib/png/include' png_LIBS='-L/opt/simulator_cairo_lib/png/lib/ -lpng16' 
```