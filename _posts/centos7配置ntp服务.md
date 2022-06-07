---
layout:     post   				    # 使用的布局（不需要改）
title:      Centos7配置ntp服务				# 标题
date:       2022-06-07 # 时间
author:     stq 					# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:		ntp						#标签
   
---
# 服务端
1.修改时区
2.安装ntp服务
```bash
yum install ntp ntpdate -y 

cat >> /etc/ntp.conf << EOF
server 0.asia.pool.ntp.org
server 1.asia.pool.ntp.org
server 2.asia.pool.ntp.org
server 3.asia.pool.ntp.org
EOF

systemctl start ntpd 
systemctl enable ntpd.service        #设置开机启动服务 
ntpdate -q 2.asia.pool.ntp.org 3.asia.pool.ntp.org #同步时间

crontab -e 
00 01 * * * root /usr/sbin/ntpdate -q 0.asia.pool.ntp.org 1.asia.pool.ntp.org #定时任务
```