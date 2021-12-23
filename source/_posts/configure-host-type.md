---
title: How to configure host type in linux
date: 2021-12-13 00:28:41
summary: 如何在linux下配置编译目标机器类型。
tags:
  - Linux
  - host type 
---

## 1. --host=i686-w64-mingw32 

```
./configure --host=i686-w64-mingw32     --prefix=/f/libiconv1.16/libiconv             CC=i686-w64-mingw32-gcc             CPPFLAGS="-I/usr/local/mingw32/include -Wall"             LDFLAGS="-L/usr/local/mingw32/lib"
```

## 2. --host=x86_64-w64-mingw32  

	PATH=/usr/local/mingw64/bin:$PATH
	export PATH

	./configure --host=x86_64-w64-mingw32 --prefix=/f//libiconv1.16/libiconv \
		CC=x86_64-w64-mingw32-gcc \
		CPPFLAGS="-I/usr/local/mingw64/include -Wall" \
		LDFLAGS="-L/usr/local/mingw64/lib"
	