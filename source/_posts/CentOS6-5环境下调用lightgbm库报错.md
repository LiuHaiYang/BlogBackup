---
title: CentOS6.5环境下调用lightgbm库报错
date: 2020-03-18 23:06:59
tags: linux
---

CentOS6.5 环境下调用lightgbm库报错: OSError: /lib64/libc.so.6: version `GLIBC_2.14' not found (required by /usr/local/python3/lib/python3.6/site-packages/lightgbm/lib_lightgbm.so)

---

搞了一晚上，网上各种方案 都不搞不定啊。

最终选用的方法(推荐)
下图是系统glibc的版本，gcc的版本是4.8.2。

`strings /lib64/libc.so.6 |grep GLIBC`

![image](https://img2018.cnblogs.com/i-beta/1885173/201912/1885173-20191202220532624-828172512.png)

红框部分需要做如下操作就可以得到，也就解决了报错。

直接使用 wget 命令下载 rpm文件并安装，如果速度太慢可以直接下载再上传。

```
wget http://copr-be.cloud.fedoraproject.org/results/mosquito/myrepo-el6/epel-6-x86_64/glibc-2.17-55.fc20/glibc-2.17-55.el6.x86_64.rpm
wget http://copr-be.cloud.fedoraproject.org/results/mosquito/myrepo-el6/epel-6-x86_64/glibc-2.17-55.fc20/glibc-common-2.17-55.el6.x86_64.rpm
wget http://copr-be.cloud.fedoraproject.org/results/mosquito/myrepo-el6/epel-6-x86_64/glibc-2.17-55.fc20/glibc-devel-2.17-55.el6.x86_64.rpm
wget http://copr-be.cloud.fedoraproject.org/results/mosquito/myrepo-el6/epel-6-x86_64/glibc-2.17-55.fc20/glibc-headers-2.17-55.el6.x86_64.rpm
 
sudo rpm -Uvh glibc-2.17-55.el6.x86_64.rpm glibc-common-2.17-55.el6.x86_64.rpm glibc-devel-2.17-55.el6.x86_64.rpm glibc-headers-2.17-55.el6.x86_64.rpm  --force --nodeps
```

nodeps的意思是忽视依赖关系
例如：rpm -ivh jdk-8u181-linux-i586.rpm --force --nodeps
 
strings /lib64/libc.so.6 |grep GLIBC