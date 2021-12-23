---
title: Build blog with hexo and github
date: 2021-12-12 18:18:45
author: cycle member
img: /medias/banner/3.jpeg
top: true
cover: true
coverImg: /medias/banner/1.jpeg
password: 8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
toc: false
mathjax: false
summary: 如何使用Hexo和github来搭建博客。本文共分5小节，第一节准备运行环境和github账户；第二节安装Hexo；如何使用hexo在第三节介绍，随后在第四节关联github pages，最后就可以访问博客了；
categories: GitHub pages blog
tags:
  - Blog
  - Hexo 
  - GitHub
---

# Build Blog with hexo and github

https://docs.github.com/en/pages/getting-started-with-github-pages


## 1. prepare
1. github user and key
2. git bash tool
3. node.js

## 2. init hexo template

1. in blank folder '/e/blog ' , open git bash ;			

		npm install -g hexo

2. init hexo

		hexo init
3. open server

		npm install -->hexo server -->hexo clean -->hexo generate -->hexo deploy

## 2.1 init hexo template

	root@QOL6GT6 MINGW64 /
	$ cd /e/blog

	root@QOL6GT6 MINGW64 /e/blog
	$ ls

	root@QOL6GT6 MINGW64 /e/blog
	$ npm install -g hexo

	added 87 packages, and audited 88 packages in 12s

	12 packages are looking for funding
		run `npm fund` for details

	1 moderate severity vulnerability

	Some issues need review, and may require choosing
	a different dependency.

	Run `npm audit` for details.

	root@QOL6GT6 MINGW64 /e/blog
	$ npm fund
	1

	root@QOL6GT6 MINGW64 /e/blog
	$ hexo init
	INFO  Cloning hexo-starter https://github.com/hexojs/hexo-starter.git
	fatal: unable to access 'https://github.com/hexojs/hexo-starter.git/': OpenSSL SSL_read: Connection was reset, errno 10054
	WARN  git clone failed. Copying data instead
	INFO  Install dependencies
	INFO  Start blogging with Hexo!

	root@QOL6GT6 MINGW64 /e/blog
	$ npm install

	up to date, audited 239 packages in 3s

	15 packages are looking for funding
		run `npm fund` for details

	1 moderate severity vulnerability

	Some issues need review, and may require choosing
	a different dependency.

	Run `npm audit` for details.

	root@QOL6GT6 MINGW64 /e/blog
	$ hexo server
	INFO  Validating config
	INFO  Start processing
	INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
	(node:10160) Warning: Accessing non-existent property 'lineno' of module exports inside circular dependency
	(Use `node --trace-warnings ...` to show where the warning was created)
	(node:10160) Warning: Accessing non-existent property 'column' of module exports inside circular dependency
	(node:10160) Warning: Accessing non-existent property 'filename' of module exports inside circular dependency
	(node:10160) Warning: Accessing non-existent property 'lineno' of module exports inside circular dependency
	(node:10160) Warning: Accessing non-existent property 'column' of module exports inside circular dependency
	(node:10160) Warning: Accessing non-existent property 'filename' of module exports inside circular dependency
	INFO  Bye!

	root@QOL6GT6 MINGW64 /e/blog
	$ hexo clean
	INFO  Validating config
	INFO  Deleted database.

	root@QOL6GT6 MINGW64 /e/blog
	$ hexo generate
	INFO  Validating config
	INFO  Start processing
	INFO  Files loaded in 120 ms
	(node:6324) Warning: Accessing non-existent property 'lineno' of module exports inside circular dependency
	(Use `node --trace-warnings ...` to show where the warning was created)
	(node:6324) Warning: Accessing non-existent property 'column' of module exports inside circular dependency
	(node:6324) Warning: Accessing non-existent property 'filename' of module exports inside circular dependency
	(node:6324) Warning: Accessing non-existent property 'lineno' of module exports inside circular dependency
	(node:6324) Warning: Accessing non-existent property 'column' of module exports inside circular dependency
	(node:6324) Warning: Accessing non-existent property 'filename' of module exports inside circular dependency
	INFO  Generated: archives/index.html
	INFO  Generated: archives/2021/index.html
	INFO  Generated: fancybox/fancybox_loading@2x.gif
	INFO  Generated: js/script.js
	INFO  Generated: css/style.css
	INFO  Generated: index.html
	INFO  Generated: fancybox/blank.gif
	INFO  Generated: fancybox/fancybox_loading.gif
	INFO  Generated: fancybox/fancybox_sprite.png
	INFO  Generated: fancybox/fancybox_sprite@2x.png
	INFO  Generated: css/fonts/fontawesome-webfont.ttf
	INFO  Generated: fancybox/fancybox_overlay.png
	INFO  Generated: fancybox/jquery.fancybox.css
	INFO  Generated: fancybox/helpers/fancybox_buttons.png
	INFO  Generated: fancybox/jquery.fancybox.pack.js
	INFO  Generated: fancybox/jquery.fancybox.js
	INFO  Generated: fancybox/helpers/jquery.fancybox-thumbs.css
	INFO  Generated: fancybox/helpers/jquery.fancybox-buttons.css
	INFO  Generated: archives/2021/12/index.html
	INFO  Generated: fancybox/helpers/jquery.fancybox-media.js
	INFO  Generated: fancybox/helpers/jquery.fancybox-thumbs.js
	INFO  Generated: fancybox/helpers/jquery.fancybox-buttons.js
	INFO  Generated: css/images/banner.jpg
	INFO  Generated: css/fonts/fontawesome-webfont.svg
	INFO  Generated: css/fonts/fontawesome-webfont.woff
	INFO  Generated: css/fonts/FontAwesome.otf
	INFO  Generated: css/fonts/fontawesome-webfont.eot
	INFO  Generated: 2021/12/12/hello-world/index.html
	INFO  28 files generated in 381 ms

	root@QOL6GT6 MINGW64 /e/blog
	$ hexo deploy
	INFO  Validating config

	root@QOL6GT6 MINGW64 /e/blog
	$ hexo server
	INFO  Validating config
	INFO  Start processing
	INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
	(node:7984) Warning: Accessing non-existent property 'lineno' of module exports inside circular dependency
	(Use `node --trace-warnings ...` to show where the warning was created)
	(node:7984) Warning: Accessing non-existent property 'column' of module exports inside circular dependency
	(node:7984) Warning: Accessing non-existent property 'filename' of module exports inside circular dependency
	(node:7984) Warning: Accessing non-existent property 'lineno' of module exports inside circular dependency
	(node:7984) Warning: Accessing non-existent property 'column' of module exports inside circular dependency
	(node:7984) Warning: Accessing non-existent property 'filename' of module exports inside circular dependency
	INFO  Good bye

	root@QOL6GT6 MINGW64 /e/blog
	$

## 3. hexo command
* hexo new "postName" #新建文章
* hexo new page "pageName" #新建页面
* hexo generate #生成静态页面至public目录
* hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
* hexo deploy #部署到GitHub
* hexo help  # 查看帮助
* hexo version  #查看Hexo的版本

## 4. deploy

1. generate ssh key and set in github.com

2. in file _config.yml, find the node:  deploy

eg.

	deploy:
		type: git
		repository: git@github.com:username/username.github.io.git
		branch: master

3. deploy

		hexo clean
		hexo deploy

## 5. visit blog

	visit url: username.github.io