---
title: matrixdb集群离线搭建
description: 
published: true
date: 2021-08-24T07:08:10.378Z
tags: Greenplum,MatrixDB,PostgreSql
editor: markdown
dateCreated: 2021-08-24T05:15:29.421Z
---

# matrixdb集群离线搭建
 由于官网的搭建文档不够全面，记录下我的搭建过程
 
 ## 软硬件资源
 ```
   硬件资源:准备六台服务器 
  192.168.1.1  mdw作为主节点
  192.168.1.2  bdw作为备份节点
  192.168.1.3  sdw1 作为数据节点
  192.168.1.4  sdw2
  192.168.1.5  sdw3
  192.168.1.6  sdw4
  
 matrixdb 官网https://www.ymatrix.cn/download
 下载matrixdb依赖 matrixdb_local_repo.tar
 matrixdb安装包 matrixdb-4.1.0.community-1.el7.x86_64.rpm
 python依赖包 pip.dep.tar
   ``` 
 ## 前置准备
 ```bash
#设置主机名 
hostnamectl set-hostname mdw 

#设置host
echo '192.168.1.1 mdw' >>/etc/hosts
echo '192.168.1.2 bdw' >>/etc/hosts
echo '192.168.1.3 sdw1' >>/etc/hosts
echo '192.168.1.4 sdw2' >>/etc/hosts
echo '192.168.1.5 sdw3' >>/etc/hosts
echo '192.168.1.6 sdw4' >>/etc/hosts
#创建用户 并配置免密登陆
adduser mxadmin
passwd mxadmin
echo 'Port 2223' >>/etc/ssh/ssh_config
echo 'StrictHostKeyChecking no' >>/etc/ssh/ssh_config 
echo 'UserKnownHostsFile /dev/null' >>/etc/ssh/ssh_config 
chown -R mxadmin /home/mxadmin
chmod 700 /home/mxadmin/.ssh/
chmod 600 /home/mxadmin/.ssh/authorized_keys
su mxadmin 
ssh-keygen #然后一路回车确定
ssh-copy-id sdw1 #输入密码确认
ssh-copy-id sdw2
ssh-copy-id sdw3
ssh-copy-id sdw4
```

## 开始安装

``` bash
#安装依赖库
	tar xf matrixdb_local_repo.tar
	cd matrixdb_local_repo
	bash create_repo.sh 
	cd ..
  
#关闭防火墙
systemctl stop firewalld.service
systemctl disable firewalld.service
vi /etc/selinux/config selinux=disabled  

#安装matridb程序
yum install --disablerepo=* --enablerepo=ymatrix matrixdb-4.1.0.community-1.el7.x86_64.rpm

#安装python依赖
source /usr/local/matrixdb/greenplum_path.sh
yum install --disablerepo=* --enablerepo=ymatrix python3-devel
tar xf pip.dep.tar
pip3 install pip.dep/*.whl


安装完毕
```
>使用浏览器访问以下图形化安装向导URL，IP为mdw服务器的IP： 
>http://<IP>:8240/
>第一步，添加节点，在文本框里输入节点的IP地址、主机名或FQDN并点击“添加节点”：
> 
> 第二步，配置数据库，选择数据库目录存储路径和segment数量。系统自动推荐空间最大的磁盘和与系统资源相匹配的segment数目，可根据具体使用场景调整。“启用数据自动镜像”决定了集群数据节点是否包含备份镜像，在生产环境中建议勾选，这样集群才是高可用的。确认后点击“下一步”：
>
> 第三步，设置密码。MatrixDB会建立mxadmin数据库管理员账户，并作为超级账户。在这个环节设置mxadmin账户的密码，然后点击“下一步”（此处设置的是数据库账户的密码，而非操作系统账户的密码）
>
> 第四步，确认部署。该步骤会列出来之前的操作的完成配置参数，确认无误后，点击“执行部署”：
> 

## 启动
>

    
   