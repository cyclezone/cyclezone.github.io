---
title: 15. compile Qt for android in Windows Environment
author: cyclezone
img: /medias/banner/3.jpeg
top: false
cover: false
coverImg: /medias/banner/1.jpeg
password: 8d969eef6
toc: false
mathjax: false
summary: compile Qt for android in Windows Environment
categories: compile Qt
tags:
  - cycle
  - Qt
date: 2021-12-19 21:00:01
---

# 15. compile Qt for android in Windows Environment

for more infomation,please visit https://doc.qt.io/qt-5/android-building.html 

## 15.1. Prepare

To build Qt for Android under a Windows environment, follow the steps below:

Preparing the Build Environment
Install the following:

* A JDK package such as AdoptOpenJDK, JDK, or OpenJDK.
* MinGW 7.3 toolchain
* Perl

Then set the respective environment variables from the Environment Variables system UI, or from the build command line prompt. For the default Command prompt:

* set JDK_ROOT=<JDK_ROOT_PATH>\bin
* set MINGW_ROOT=<MINGW_ROOT_PATH>\bin
* set PERL_ROOT=<PERL_ROOT_PATH>\bin
* set PATH=%MINGW_ROOT%;%PERL_ROOT%;%JDK_ROOT%:%PATH%

Then, in the command line prompt, verify that:

```
  where gcc.exe
  The command should list gcc.exe under the path <MINGW_ROOT> first.

  where mingw32-make.exe
  The command should list mingw32-make.exe under the path <MINGW_ROOT> first.

  where javac.exe
  The command should list javac.exe under the path <JDK_ROOT> first.
```

* Note: JDK 11 or earlier must be used to properly build Qt for Android.

* Note: Qt for Android does not support building with Microsoft Visual C++ (MSVC), we only support building with MinGW.

* If you have downloaded the source code archive from Qt Downloads, then unpack the archive if you have not done so already.  qt-everywhere-src-%VERSION%.tar.xz package

   
## 15.2. Compile


1. save the following compile script into file *.bat
2. open cmd console,open the source code path,
3. run the *.bat created in step 1;
   
### compile script
   
```
  :::::::::::::::::::::::::::::::::::::compile script ::::::::::::::::::::::::::::::::::::
  :::::::::::::::::::::::::::::::::::::arm64-v8a  armeabi-v7a  armeabi::::::::::::::::::::

  @echo %PATH%

  set "ANDROID_API_VERSION=android-21"
  set "ANDROID_SDK_ROOT=D:\android\program\adt\sdk"
  set "ANDROID_TARGET_ARCH=arm64-v8a"
  set "ANDROID_BUILD_TOOLS_REVISION=21.1.2"
  set "ANDROID_NDK_PATH=D:\android\program\adt\ndk-r20b"
  set "ANDROID_TOOLCHAIN_VERSION=4.9"
  set "ANDROID_NDK_HOST=windows-x86_64"
  set "BUILD_PATH=E:\Qt\build"
  set "PREFIX_PATH=E:\Qt\install"

  set JDK_ROOT=D:\Pragram\Java\jdk1.8.0_162\bin
  set MINGW_ROOT=E:\SoftWare\mingw32\bin
  set PERL_ROOT=C:\Strawberry\perl\bin
  set PATH=%MINGW_ROOT%;%PERL_ROOT%;%JDK_ROOT%:%PATH%

  ::configure.bat -prefix F:\Qt\build -platform win32-g++ -opengl es2 -xplatform android-g++ -android-ndk %ANDROID_NDK_PATH% -android-sdk %ANDROID_SDK_ROOT% -android-toolchain-version 4.9 -nomake tests -nomake examples
  ::configure.bat  -developer-build -platform win32-g++ -opengl dynamic -xplatform android-g++ -android-ndk %ANDROID_NDK_PATH% -android-sdk %ANDROID_SDK_ROOT% -android-toolchain-version 4.9 -nomake tests -nomake examples

  ::configure.bat -platform win32-g++ -opengl es2 -xplatform android-g++ -android-ndk %ANDROID_NDK_PATH% -android-sdk %ANDROID_SDK_ROOT% -android-toolchain-version 4.9 -android-ndk-host windows-x86_64 -android-ndk-platform android-21 -opensource -confirm-license -nomake tests -nomake examples -static
  ::configure.bat -opengl dynamic -no-opengl

  pause

  mkdir %BUILD_PATH%
  mkdir %PREFIX_PATH%
  mkdir %BUILD_PATH%\%ANDROID_TARGET_ARCH%
  mkdir %PREFIX_PATH%\%ANDROID_TARGET_ARCH%
  cd %BUILD_PATH%\%ANDROID_TARGET_ARCH%

  ..\..\5.12.5\configure.bat -platform win32-g++ -opengl es2 -xplatform android-clang -android-ndk %ANDROID_NDK_PATH% -android-sdk %ANDROID_SDK_ROOT% -android-toolchain-version 4.9 -android-ndk-host windows-x86_64 -android-ndk-platform android-21 -prefix %PREFIX_PATH%\%ANDROID_TARGET_ARCH% -opensource -confirm-license -nomake tests -nomake examples -shared

  ::mingw32-make -j4
  mingw32-make

  pause
  :::::::::::::::::::::::::::::::::::::compile script::::::::::::::::::::::::::::::::::::
```
## 15.3 some problems

### Android linker: undefined reference to bsd_signal

* /android/ndk/platforms/android-9/arch-arm/usr/include/signal.h:113: error: undefined reference to 'bsd_signal'
  
Till android-19 inclusive NDK-s signal.h declared bsd_signal extern and signal was an inline calling bsd_signal.

Starting with android-21 signal is an extern and bsd_signal is not declared at all.

What's interesting, bsd_signal was still available as a symbol in NDK r10e android-21 libc.so (so there were no linking errors if using r10e), but is not available in NDK r11 and up.

Removing of bsd_signal from NDK-s android-21+ libc.so results in linking errors if code built with android-21+ is linked with static libs built with lower NDK levels that call signal or bsd_signal. Most popular library which calls signal is OpenSSL.

WARNING: Building those static libs with android-21+ (which would put signal symbol directly) would link fine, but would result in *.so failing to load on older Android OS devices due to signal symbol not found in theirs libc.so.

Therefore it's better to stick with <=android-19 for any code that calls signal or bsd_signal.

To link a library built with android-21 I ended up declaring a bsd_signal wrapper which would call bsd_signal from libc.so (it's still available in device's libc.so, even up to Android 7.0).

```
  #if (__ANDROID_API__ > 19)
  #include <android/api-level.h>
  #include <android/log.h>
  #include <signal.h>
  #include <dlfcn.h>

  extern "C" {
    typedef __sighandler_t (*bsd_signal_func_t)(int, __sighandler_t);
    bsd_signal_func_t bsd_signal_func = NULL;

    __sighandler_t bsd_signal(int s, __sighandler_t f) {
      if (bsd_signal_func == NULL) {
        // For now (up to Android 7.0) this is always available 
        bsd_signal_func = (bsd_signal_func_t) dlsym(RTLD_DEFAULT, "bsd_signal");

        if (bsd_signal_func == NULL) {
          // You may try dlsym(RTLD_DEFAULT, "signal") or dlsym(RTLD_NEXT, "signal") here
          // Make sure you add a comment here in StackOverflow
          // if you find a device that doesn't have "bsd_signal" in its libc.so!!!

          __android_log_assert("", "bsd_signal_wrapper", "bsd_signal symbol not found!");
        }
      }

      return bsd_signal_func(s, f);
    }
  }
  #endif
```

PS. Looks like the bsd_signal symbol will be brought back to libc.so in NDK r13:
for more information ,please visit 
 * https://stackoverflow.com/questions/36746904/android-linker-undefined-reference-to-bsd-signal
   
 * https://github.com/android-ndk/ndk/issues/160#issuecomment-236295994


