---
title: 32.How to Install Gitlab for linux
author: cyclezone
img: /medias/banner/3.jpeg
top: true
cover: true
coverImg: /medias/banner/2.jpeg
password: 8d969eef6
toc: true
mathjax: false
summary: How to Install Gitlab for linux
categories: Gitlab
tags:
  - cycle
  - gitlab
date: 2021-12-23 23:21:32
---

# 32.Install Gitlab for linux

## About gitlab

```
/etc/gitlab/gitlab.rb          #gitlab配置文件
/opt/gitlab                    #gitlab的程序安装目录
/var/opt/gitlab                #gitlab目录数据目录
/var/opt/gitlab/git-data       #存放仓库数据
gitlab-ctl reconfigure         #重新加载配置
gitlab-ctl status              #查看当前gitlab所有服务运行状态
gitlab-ctl stop                #停止gitlab服务
gitlab-ctl stop nginx          #单独停止某个服务
gitlab-ctl tail                #查看所有服务的日志

Gitlab的服务构成：
nginx：                        静态web服务器
gitlab-workhorse               轻量级反向代理服务器
logrotate                      日志文件管理工具
postgresql                     数据库
redis                          缓存数据库
sidekiq                        用于在后台执行队列任务（异步执行）
```
##  Install steps

1. prepare 
   * centos 7 , 4G memory
   * gitlab-ce-10.2.2-ce
   * xftp ,xshell
  
2. visit https://mirrors.tuna.tsinghua.edu.cn/gitlab-ee/yum/el7/ ,
     
	 get the install package gitlab-ee-14.5.1-ee.0.el7.x86_64.rpm
    
	 then copy package to the target folder /user/gitlab in centos7 ;

3. install dependency and package 
    ```
	yum install -y postfix sshd policycoreutils-python

	cd /user/gitlab

	rpm -ivh gitlab-ee-14.5.1-ee.0.el7.x86_64.rpm
    ```
4. configure
    ```
	[root@gitlab ~]# vim /etc/gitlab/gitlab.rb     
	external_url 'http://192.168.1.21'        
	[root@gitlab ~]# gitlab-ctl reconfigure 
    ```
5. visit gitlab 
    ```
	in remote machine visit the ip configured in step 4;
	http://192.168.1.21
    ```
6. change ui chinese

	1、dowload the service pack 

	```
	[root@gitlab ~]# git clone https://gitlab.com/xhang/gitlab.git
	[root@gitlab ~]# cd gitlab    
	```
	2、find the branch version

	```
	[root@gitlab ~]# git branch -a
	```
	3、compare version ,generate the service pack

	```
	[root@gitlab ~]# git diff remotes/origin/10-2-stable remotes/origin/10-2-stable-zh > /tmp/10.2.2-zh.diff
	```
	4、stop gitlab service

	```
	[root@gitlab ~]# gitlab-ctl stop
	```
	5、patch the service pack

	```
	[root@gitlab ~]# patch -d /opt/gitlab/embedded/service/gitlab-rails -p1 < /tmp/10.2.2-zh.diff
	```
	6、start and reconfigure

	```
	[root@gitlab ~]# gitlab-ctl start
	[root@gitlab ~]# gitlab-ctl reconfigure
	```

7.  fire wall
   
      if cannot visit gitlab,please close fire wall or open port 80;

	```    
	service iptables stop

	vi /etc/sysconfig/iptables

	-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT

	service iptables restart
	```

8. config email
		
		vim /etc/gitlab/gitlab.rb  
		external_url 'http://192.168.1.21' 
		### Email Settings
		gitlab_rails['gitlab_email_enabled'] = true
		gitlab_rails['gitlab_email_from'] = 'huawei@qq.com'
		gitlab_rails['gitlab_email_display_name'] = 'huawei'

		### GitLab email server settings
		### here use the qq email,if use other email server,modify the smtp address
		gitlab_rails['smtp_enable'] = true
		gitlab_rails['smtp_address'] = "smtp.exmail.qq.com"
		gitlab_rails['smtp_port'] = 465
		gitlab_rails['smtp_user_name'] = "huawei@qq.com"
		gitlab_rails['smtp_password'] = "123456"
		gitlab_rails['smtp_authentication'] = "login"
		gitlab_rails['smtp_enable_starttls_auto'] = true
		gitlab_rails['smtp_tls'] = true
		
			

9. login with root
		

		username: root
		password: QjD1J3CZW3XOnJnWX5ieUt7V6GoSJjskvQiBNPnxBDc=
		password get from file 'initial_root_password' which located in ' /etc/gitlab'

		[root@localhost gitlab]# pwd
		/etc/gitlab
		[root@localhost gitlab]# ls
		gitlab.rb  gitlab-secrets.json  initial_root_password  trusted-certs
		[root@localhost gitlab]# cat initial_root_password 
		# WARNING: This value is valid only in the following conditions
		#          1. If provided manually (either via `GITLAB_ROOT_PASSWORD` environment variable or via `gitlab_rails['initial_root_password']` setting in `gitlab.rb`, it was provided before database was seeded for the first time (usually, the first reconfigure run).
		#          2. Password hasn't been changed manually, either via UI or via command line.
		#
		#          If the password shown here doesn't work, you must reset the admin password following https://docs.gitlab.com/ee/security/reset_user_password.html#reset-your-root-password.

		Password: QjD1J3CZW3XOnJnWX5ieUt7V6GoSJjskvQiBNPnxBDc=

		# NOTE: This file will be automatically deleted in the first reconfigure run after 24 hours.
		[root@localhost gitlab]#
		
	
