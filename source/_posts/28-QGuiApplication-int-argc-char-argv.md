---
title: '28.QGuiApplication(int &argc, char **argv)'
author: cyclezone
img: /medias/banner/3.jpeg
top: false
cover: false
coverImg: /medias/banner/1.jpeg
password: 8d969eef6
toc: false
mathjax: false
summary: 'QGuiApplication(int &argc, char **argv)'
categories: cycle zone
tags:
  - cycle
date: 2021-12-22 23:17:50
---

# 28.QGuiApplication(int &argc, char **argv)

 for more information, please visit 

   https://doc.qt.io/qt-5/qguiapplication.html#QGuiApplication

   https://stackoverflow.com/questions/17979185/qt-5-1-qapplication-without-display-qxcbconnection-could-not-connect-to-displ

Initializes the window system and constructs an application object with argc command line arguments in argv.

Warning: The data referred to by argc and argv must stay valid for the entire lifetime of the QGuiApplication object. In addition, argc must be greater than zero and argv must contain at least one valid character string.

The global qApp pointer refers to this application object. Only one application object should be created.

This application object must be constructed before any paint devices (including pixmaps, bitmaps etc.).

Note: argc and argv might be changed as Qt removes command line arguments that it recognizes.

## Supported Command Line Options

All Qt programs automatically support a set of command-line options that allow modifying the way Qt will interact with the windowing system. Some of the options are also accessible via environment variables, which are the preferred form if the application can launch GUI sub-processes or other applications (environment variables will be inherited by child processes). When in doubt, use the environment variables.

### The options currently supported are the following:

```
-platform platformName[:options], specifies the Qt Platform Abstraction (QPA) plugin.
Overrides the QT_QPA_PLATFORM environment variable.
```

```
-platformpluginpath path, specifies the path to platform plugins.
Overrides the QT_QPA_PLATFORM_PLUGIN_PATH environment variable.

-platformtheme platformTheme, specifies the platform theme.
Overrides the QT_QPA_PLATFORMTHEME environment variable.

-plugin plugin, specifies additional plugins to load. The argument may appear multiple times.
Concatenated with the plugins in the QT_QPA_GENERIC_PLUGINS environment variable.

-qmljsdebugger=, activates the QML/JS debugger with a specified port. The value must be of format port:1234[,block], where block is optional and will make the application wait until a debugger connects to it.
-qwindowgeometry geometry, specifies window geometry for the main window using the X11-syntax. For example: -qwindowgeometry 100x100+50+50
-qwindowicon, sets the default window icon
-qwindowtitle, sets the title of the first window
-reverse, sets the application's layout direction to Qt::RightToLeft. This option is intended to aid debugging and should not be used in production. The default value is automatically detected from the user's locale (see also QLocale::textDirection()).
-session session, restores the application from an earlier session.
The following standard command line options are available for X11:

-display hostname:screen_number, switches displays on X11.
Overrides the DISPLAY environment variable.

-geometry geometry, same as -qwindowgeometry.
Platform-Specific Arguments
You can specify platform-specific arguments for the -platform option. Place them after the platform plugin name following a colon as a comma-separated list. For example, -platform windows:dialogs=xp,fontengine=freetype.
```


### platformName : const QString
This property holds the name of the underlying platform plugin.

The QPA platform plugins are located in qtbase\src\plugins\platforms. At the time of writing, the following platform plugin names are supported:
```
android
cocoa is a platform plugin for macOS.
directfb
eglfs is a platform plugin for running Qt5 applications on top of EGL and OpenGL ES 2.0 without an actual windowing system (like X11 or Wayland). For more information, see EGLFS.
ios (also used for tvOS)
kms is an experimental platform plugin using kernel modesetting and DRM (Direct Rendering Manager).
linuxfb writes directly to the framebuffer. For more information, see LinuxFB.
minimal is provided as an examples for developers who want to write their own platform plugins. However, you can use the plugin to run GUI applications in environments without a GUI, such as servers.
minimalegl is an example plugin.
offscreen
openwfd
qnx
windows
wayland is a platform plugin for modern Linux desktops and some embedded systems.
xcb is the X11 plugin used on regular desktop Linux platforms.
For more information about the platform plugins for embedded Linux devices, see Qt for Embedded Linux.

Access functions:

QString	platformName()
```

### The following parameters are available for -platform windows:

```
altgr, detect the key AltGr found on some keyboards as Qt::GroupSwitchModifier (since Qt 5.12).
darkmode=[1|2] controls how Qt responds to the activation of the Dark Mode for applications introduced in Windows 10 1903 (since Qt 5.15).
A value of 1 causes Qt to switch the window borders to black when Dark Mode for applications is activated and no High Contrast Theme is in use. This is intended for applications that implement their own theming.

A value of 2 will in addition cause the Windows Vista style to be deactivated and switch to the Windows style using a simplified palette in dark mode. This is currently experimental pending the introduction of new style that properly adapts to dark mode.

dialogs=[xp|none], xp uses XP-style native dialogs and none disables them.
dpiawareness=[0|1|2] Sets the DPI awareness of the process (see High DPI Displays, since Qt 5.4).
fontengine=freetype, uses the FreeType font engine.
menus=[native|none], controls the use of native menus.
Native menus are implemented using Win32 API and are simpler than QMenu-based menus in for example that they do allow for placing widgets on them or changing properties like fonts and do not provide hover signals. They are mainly intended for Qt Quick. By default, they will be used if the application is not an instance of QApplication or for Qt Quick Controls 2 applications (since Qt 5.10).

nocolorfonts Turn off DirectWrite Color fonts (since Qt 5.8).
nodirectwrite Turn off DirectWrite fonts (since Qt 5.8).
nomousefromtouch Ignores mouse events synthesized from touch events by the operating system.
nowmpointer Switches from Pointer Input Messages handling to legacy mouse handling (since Qt 5.12).
reverse Activates Right-to-left mode (experimental). Windows title bars will be shown accordingly in Right-to-left locales (since Qt 5.13).
tabletabsoluterange=<value> Sets a value for mouse mode detection of WinTab tablets (Legacy, since Qt 5.3).
The following parameter is available for -platform cocoa (on macOS):

fontengine=freetype, uses the FreeType font engine.
For more information about the platform-specific arguments available for embedded Linux platforms, see Qt for Embedded Linux.
```
