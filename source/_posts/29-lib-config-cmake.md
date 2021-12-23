---
title: 29.lib-config.cmake
author: cyclezone
img: /medias/banner/3.jpeg
top: false
cover: false
coverImg: /medias/banner/1.jpeg
password: 8d969eef6
toc: false
mathjax: false
summary: lib-config.cmake
categories: cycle zone
tags:
  - cycle
date: 2021-12-22 23:18:39
---

# 29.lib-config.cmake

*  set(xxx_FOUND TRUE)
*  use xxx_INCLUDE_DIRS and xxx_LIBRARIES in CMakeLists.txt
*  set(xxx_INCLUDE_DIRS ${MYSQLCPPCONN_INCLUDE_DIR} )
*  set(xxx_LIBRARIES ${MYSQLCPPCONN_LIBRARY} )

 ##  pslib-config.cmake
```
get_filename_component(_DIR "${CMAKE_CURRENT_LIST_FILE}" PATH)

if(NOT PSLIB_FIND_COMPONENTS)
    set(PSLIB_FIND_COMPONENTS pslib pslib)
    if(PSLIB_FIND_REQUIRED)
        set(PSLIB_FIND_REQUIRED_pslib TRUE)
    endif()

    set(PSLIB_FOUND TRUE)
endif()

set(PSLIB_INCLUDE_DIRS ${_DIR}/../../include)
set(PSLIB_LIBRARIES)
if (EXISTS ${_DIR}/../../lib/pslib.a)
    list(APPEND PSLIB_LIBRARIES optimized ${_DIR}/../../lib/pslib.a)
endif()
if (EXISTS ${_DIR}/../../debug/lib/pslib.a)
    list(APPEND PSLIB_LIBRARIES debug ${_DIR}/../../debug/lib/pslib.a)
endif()
if (EXISTS ${_DIR}/../../lib/pslib.lib)
    list(APPEND PSLIB_LIBRARIES optimized ${_DIR}/../../lib/pslib.lib)
endif()
if (EXISTS ${_DIR}/../../debug/lib/pslib.lib)
    list(APPEND PSLIB_LIBRARIES debug ${_DIR}/../../debug/lib/pslib.lib)
endif()
```
 ##  mysqlcppconn-config.cmake
```
# Name: <Name>Config.cmake  or <lower name>-config.cmake
# mysqlcppconn-config.cmake or MYSQLCPPCONNConfig.cmake  
# similar to OpenCVConfig.cmake   

# Tips for MYSQLCPPCONN_ROOT_DIR
# use "C:/Program Files/MySQL/Connector.C++ 1.1", otherwise cmake-gui can not auto find include and library

set(MYSQLCPPCONN_FOUND TRUE) # auto 
set(MYSQLCPPCONN_ROOT_DIR "C:/Program Files/MySQL/Connector.C++ 1.1")

find_path(MYSQLCPPCONN_INCLUDE_DIR NAMES cppconn/driver.h PATHS "${MYSQLCPPCONN_ROOT_DIR}/include") 
mark_as_advanced(MYSQLCPPCONN_INCLUDE_DIR) # show entry in cmake-gui

find_library(MYSQLCPPCONN_LIBRARY NAMES mysqlcppconn.lib PATHS "${MYSQLCPPCONN_ROOT_DIR}/lib/opt") 
mark_as_advanced(MYSQLCPPCONN_LIBRARY) # show entry in cmake-gui

# use xxx_INCLUDE_DIRS and xxx_LIBRARIES in CMakeLists.txt
set(MYSQLCPPCONN_INCLUDE_DIRS ${MYSQLCPPCONN_INCLUDE_DIR} )
set(MYSQLCPPCONN_LIBRARIES ${MYSQLCPPCONN_LIBRARY} )

# cmake entry will be saved to build/CMakeCache.txt 

message( "mysqlcppconn-config.cmake " ${MYSQLCPPCONN_ROOT_DIR})
```
## halcon-config.cmake
```
# halcon-config.cmake or HALCONConfig.cmake  

set(HALCON_FOUND TRUE) # auto 
set(HALCON_ROOT_DIR E:/git/car/windows/lib/halcon)

find_path(HALCON_INCLUDE_DIR NAMES halconcpp/HalconCpp.h PATHS "${HALCON_ROOT_DIR}/include") 
mark_as_advanced(HALCON_INCLUDE_DIR) # show entry in cmake-gui

find_library(HALCON_LIBRARY NAMES halconcpp.lib PATHS "${HALCON_ROOT_DIR}/lib/x64-win64") 
mark_as_advanced(HALCON_LIBRARY) # show entry in cmake-gui

# use xxx_INCLUDE_DIRS and xxx_LIBRARIES in CMakeLists.txt
set(HALCON_INCLUDE_DIRS ${HALCON_INCLUDE_DIR} )
set(HALCON_LIBRARIES ${HALCON_LIBRARY} )

message( "halcon-config.cmake " ${HALCON_ROOT_DIR})
```
## usage
```
find_package(HALCON REQUIRED) # user-defined
include_directories(${HALCON_INCLUDE_DIRS})

find_package(MYSQLCPPCONN REQUIRED) # user-defined
include_directories(${MYSQLCPPCONN_INCLUDE_DIRS})

target_link_libraries(demo ${HALCON_LIBRARIES} ${MYSQLCPPCONN_LIBRARIES})
```

## cmake-gui entry
1.  start cmake-gui, and at first,we should set

* HALCON_DIR = E:/git/car/share/cmake-3.10/Modules
* MYSQLCPPCONN_DIR = E:/git/car/share/cmake-3.10/Modules
2.  then configure

  * HALCON_INCLUDE_DIR and HALCON_LIBRARY will be found.
  * MYSQLCPPCONN_INCLUDE_DIR and MYSQLCPPCONN_LIBRARY will be found.