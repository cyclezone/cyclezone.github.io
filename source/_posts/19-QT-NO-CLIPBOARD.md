---
title: 19. QT_NO_CLIPBOARD
author: cyclezone
img: /medias/banner/3.jpeg
top: false
cover: false
coverImg: /medias/banner/1.jpeg
password: 8d969eef6
toc: false
mathjax: false
summary: QT_NO_CLIPBOARD
categories: cycle zone
tags:
  - cycle
date: 2021-12-19 21:12:50
---


# 19. QT_NO_CLIPBOARD

## 19.1 problem description


    void test()
    {
      int argc = 1; 
      char* argv[] = {"libnative-lib.so"};
      QApplication  application(argc,argv);
    }


the function test() is broken when init the resources in Android Environment,

when debug that funtion 'QApplication  application(argc,argv);'

step into when hit the following function which has the Macro 'QT_NO_CLIPBOARD';

    #ifndef QT_NO_CLIPBOARD
        m_androidPlatformClipboard = new QAndroidPlatformClipboard();
    #endif

continue to step into 'new QAndroidPlatformClipboard()';
some jni methods has been called ,but we cannot find the method,so program  sink without any return ;

## 19.2 method


1. to find the method is the essential way;
2. but here we use another way in case that we have no so much information about the clipboard jni method;
   
   So if we do not call that logic function,just pass over it, the program do not sink;

### 19.2.1 HOW to pass over it?

we can define the Macro 'QT_NO_CLIPBOARD' to pass over it;


next we exam the method;

1. search the macro 'QT_NO_CLIPBOARD' in the source code folder;
   we get many hits in serveral modules ,such as gui | android platform | widget | modbus | assitant tools;
   but there is no hit about 'define QT_NO_CLIPBOARD';
   WHERE to deine the macro?

2. check some  *.h or *.cpp files in which hit the macro;
   find the common includes, '#include <QtGui/private/qtguiglobal_p.h>'
   
   in qtguiglobal_p.h ,then locate :

        #include <QtGui/qtguiglobal.h>
        #include <QtCore/private/qglobal_p.h>
        #include <QtGui/private/qtgui-config_p.h>
   
   in  qtguiglobal.h,locate:

        #include <QtCore/qglobal.h>
        #include <QtGui/qtgui-config.h>

	in qglobal.h or qglobal_p.h, we cannot find any macro defination;
	 
	for qtgui-config_p.h or qtgui-config.h, firstly we cannot find them in the source code folder;
	we find them in the compile folder which has been created when run the configure command;
	in qtgui-config_p.h or qtgui-config.h, we can find many macro definition;

3. we edit the file 'qtgui-config_p.h' by appending the definition of the macro  QT_NO_CLIPBOARD ;
   then compile the source code again and test the code;
   OK,we success; prove that that method is OK;

4. But  WHY we need edit the file?




### 19.2.2   WHY we need edit the file?
   is there another way to define the macro?

1. in file qtgui-config.h, when we search key words 'clipboard' ,
   we find '#define QT_FEATURE_clipboard 1';
   
   so what create the file qtgui-config.h and define FEATURE_clipboard?

2. in the folder which locate the file qtgui-config.h, we find file qtgui-config.pri;

```
QT.gui.enabled_features = accessibility action opengles2 clipboard colornames cssparser cursor desktopservices imageformat_xpm draganddrop opengl imageformatplugin highdpiscaling im image_heuristic_mask image_text imageformat_bmp imageformat_jpeg imageformat_png imageformat_ppm imageformat_xbm movie pdf picture sessionmanager shortcut standarditemmodel systemtrayicon tabletevent texthtmlparser textodfwriter validator vulkan whatsthis wheelevent
QT.gui.disabled_features = angle combined-angle-lib dynamicgl opengles3 opengles31 opengles32 openvg
QT.gui.QT_CONFIG = accessibility action opengles2 clipboard colornames cssparser cursor desktopservices imageformat_xpm draganddrop opengl egl freetype imageformatplugin harfbuzz highdpiscaling ico im image_heuristic_mask image_text imageformat_bmp imageformat_jpeg imageformat_png imageformat_ppm imageformat_xbm movie pdf picture sessionmanager shortcut standarditemmodel systemtrayicon tabletevent texthtmlparser textodfwriter validator whatsthis wheelevent

```
in file qtgui-config.pri,clipboard is the enabled feature;
just list above;	

3. so what create the file qtgui-config.pri? How to make clipboard disabled ?

###	 19.2.3 How to make clipboard disabled ?

1. in source code folder, search key words 'clipboard' ,
2. locate file 'configure.json'; the configure script use this json file to configure;
3. in json file,
   CHANGE from 

   ```
	"clipboard": {
		"label": "QClipboard",
		"purpose": "Provides cut and paste operations.",
		"section": "Kernel",
		"condition": "!config.integrity && !config.qnx",
		"output": [ "publicFeature", "feature" ]
	},
	```

   into

	```
	"clipboard": {
		"label": "QClipboard",
		"purpose": "Provides cut and paste operations.",
		"section": "Kernel",
		"autoDetect": "config.msvc",
		"condition": "!config.integrity && !config.qnx",
		"output": [ "publicFeature", "feature" ]
	},
	```
  
4. save the changes and reconfigure,
   locate the file qtgui-config.h , there is no QT_FEATURE_clipboard defination;
   but we find '#define QT_NO_CLIPBOARD ';

5. bingo! we have made  FEATURE clipboard disabled;

6. recompile the source code and retest the demo;OK;
   
