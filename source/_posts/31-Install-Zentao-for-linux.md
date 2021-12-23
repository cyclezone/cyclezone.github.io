---
title: 31.How to Install Zentao for linux
author: cyclezone
img: /medias/banner/3.jpeg
top: false
cover: true
coverImg: /medias/banner/3.jpeg
password: 8d969eef6
toc: false
mathjax: false
summary: How to Install Zentao for linux
categories: Zentao
tags:
  - cycle
  - Zentao
date: 2021-12-23 23:20:16
---

# 31.Install Zentao for linux

##  Install steps

* 1. visit https://www.zentao.net/dynamic/zentaopms.pro10.3.1-80434.html ,

      get the linux install package,eg. 'ZenTaoPMS.15.7.1.zbox_64.tar.gz '
* 2. copy the package file into linux file folder,such as ' /user/zen '
  
* 3. open folder where package located, 'cd /user/zen '
  
* 4. unzip the file to target folder  '/opt/'; 
    
			 sudo tar -zxvf  ZenTaoPMS.15.7.1.zbox_64.tar.gz -C /opt

	  then in folder ' /opt/zbox ',we find ./zbox entry;

* 5. set port number for mysql and apache;
     
			cd /opt/zbox 
			./zbox  -mp 3307 
			./zbox  -ap 9091 
	 mp for mysql port;  ap for apache port
	 
* 6. add user into DataBase
     
	     cd /opt/zbox/auth
	     sh adduser.sh 

		 This tool is used to add user to access adminer
		 Account: root
		 Password: Adding password for user root

* 7. start mysql and apache
          
		 cd /opt/zbox
		 ./zbox start

* 8. visit 'localhost:9091 ' in local machine or
    
	 visit '192.168.10.50:9091 ' in remote machine;

	 then login and use it;

	 default user :   admin 

	 default key :    123456

* 9. when no longer use it,stop it;
     when reboot,restart it;

		 cd /opt/zbox
		 ./zbox stop
		 ./zbox restart	 
		 
* 10. if cannot visit，add port into firewall,if the firewall is active
      
			[root@localhost zbox]# systemctl status firewalld
			● firewalld.service - firewalld - dynamic firewall daemon
			   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
			   Active: active (running) since 日 2021-12-05 11:36:48 CST; 1h 47min ago
				 Docs: man:firewalld(1)
			 Main PID: 846 (firewalld)
				Tasks: 2
			   CGroup: /system.slice/firewalld.service
					   └─846 /usr/bin/python2 -Es /usr/sbin/firewalld --nofork --nopid

			12月 05 11:36:46 localhost.localdomain systemd[1]: Starting firewalld - dynam...
			12月 05 11:36:48 localhost.localdomain systemd[1]: Started firewalld - dynami...
			Hint: Some lines were ellipsized, use -l to show in full.
			[root@localhost zbox]# firewall-cmd --list-ports
			80/tcp 8081/tcp
			[root@localhost zbox]# firewall-cmd --zone=public --add-port=9091/tcp --permanentsuccess
			[root@localhost zbox]# firewall-cmd --list-ports
			80/tcp 8081/tcp
			[root@localhost zbox]# reboot  
