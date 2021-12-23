---
title: 21. Init Qt resource
author: cyclezone
img: /medias/banner/3.jpeg
top: false
cover: false
coverImg: /medias/banner/1.jpeg
password: 8d969eef6
toc: false
mathjax: false
summary: init Qt resource
categories: cycle zone
tags:
  - cycle
  - Qt
date: 2021-12-19 21:18:19
---


# 21. Init Qt resource


    public class QtEnvironment {
        public static int Init(Activity activity) {
            String rootPath = android.os.Environment.getExternalStorageDirectory().getAbsolutePath();
            String apkPath = activity.getApplicationInfo().publicSourceDir;

            String nativeLibraryPrefix = activity.getApplicationInfo().nativeLibraryDir + "/";

            DexClassLoader classLoader = new DexClassLoader("", // .jar/.apk files
                    activity.getDir("outdex", Context.MODE_PRIVATE).getAbsolutePath(), // directory where optimized DEX files should be written.
                    null, // libs folder (if exists)
                    activity.getClassLoader());

            QtNative.setClassLoader(classLoader);

            String[] qtLibs = {"Qt5Core","Qt5Gui","Qt5Widgets","qtforandroid"};
            ArrayList<String> libraryList = new ArrayList<String>();

            String libPrefix = nativeLibraryPrefix + "lib";
            for (int i = 0; i < qtLibs.length; i++)
                libraryList.add(libPrefix + qtLibs[i] + ".so");
            QtNative.loadQtLibraries(libraryList);

            return 1;
        }

    }
