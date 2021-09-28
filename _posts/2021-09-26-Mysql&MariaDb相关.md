---
layout:     post   				    # 使用的布局（不需要改）
title:      Mysql&MariaDb相关				# 标题 
date:       2021-09-26 # 时间
author:     stq 					# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:		mysql						#标签
   
---
# 使用Navicat执行大sql文件超时报错
是因为客户端/服务器之间通信的缓存区的的最大值为64M
```mysql
#重新设置缓冲区大小即可 // 26843545=256*1024*1024
set global max_allowed_packet=2684354566;
```