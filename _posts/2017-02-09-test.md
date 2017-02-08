---
layout: post
#标题配置
title:  test
#时间配置
date:   2017-02-09 03:14:00 +0800
#大类配置
categories: document
#小类配置
tag: 教程
---

* content
{:toc}

`注意：使用 su 或者 sudo 超级管理员权限`



### PHP
---

```liunx
[root@VM_77_151_centos ~]# wget http://php.net/get/php-7.1.1.tar.gz/from/a/mirror
[root@VM_77_151_centos ~]# tar -zxvf mirror
[root@VM_77_151_centos ~]# yum install gcc gcc++ libxml2-devel  #安装依赖
[root@VM_77_151_centos ~]# cd php-7.1.1
[root@VM_77_151_centos php-7.1.1]# ./configure --prefix=/usr/local/php --enable-fpm 
[root@VM_77_151_centos php-7.1.1]# make
[root@VM_77_151_centos php-7.1.1]# make install
```

### MySQL
---
```liunx
[root@VM_77_151_centos ~]# wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.17.tar.gz
[root@VM_77_151_centos ~]# tar -zxvf mysql-5.7.17.tar.gz
[root@VM_77_151_centos ~]# yum install cmake gcc-c++ ncurses-devel perl-Data-Dump boost boost-doc boost-devel  #安装依赖和工具
[root@VM_77_151_centos ~]# cd cd mysql-5.7.17
[root@VM_77_151_centos mysql-5.7.17]# cmake \
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
-DMYSQL_DATADIR=/mydata/mysql/data \
-DSYSCONFDIR=/etc \
-DMYSQL_USER=mysql \
-DWITH_MYISAM_STORAGE_ENGINE=1 \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \
-DWITH_MEMORY_STORAGE_ENGINE=1 \
-DWITH_READLINE=1 \
-DMYSQL_UNIX_ADDR=/var/run/mysql/mysql.sock  \
-DMYSQL_TCP_PORT=3306 \
-DENABLED_LOCAL_INFILE=1 \
-DENABLED_DOWNLOADS=1 \
-DWITH_PARTITION_STORAGE_ENGINE=1 \
-DEXTRA_CHARSETS=all \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci \
-DWITH_DEBUG=0 \
-DMYSQL_MAINTAINER_MODE=0 \
-DWITH_SSL:STRING=bundled \
-DWITH_ZLIB:STRING=bundled \

# 如果出现报这个（boost）错误
-- Download failed, error: 
CMake Error at cmake/boost.cmake:194 (MESSAGE):
You can try downloading
http://sourceforge.net/projects/boost/files/boost/1.59.0/boost_1_59_0.tar.gz
manually using curl/wget or a similar tool

# 在 cmake 后面加上（# 号去掉）
#-DDOWNLOAD_BOOST=1 \
#-DWITH_BOOST=/usr/share/doc/boost-doc-1.59.0/ \
#-DOWNLOAD_BOOST_TIMEOUT=3600

# 或者
[root@VM_77_151_centos mysql-5.7.17]# wget http://sourceforge.net/projects/boost/files/boost/1.59.0/boost_1_59_0.tar.gz
[root@VM_77_151_centos mysql-5.7.17]# mkdir /usr/share/doc/boost-doc-1.59.0
[root@VM_77_151_centos mysql-5.7.17]# cp ./boost_1_59_0.tar.gz /usr/share/doc/boost-doc-1.59.0/

[root@VM_77_151_centos mysql-5.7.17]# make

# 如果编译过程出现以下错误，内存不足，可以建立swap分区解决
c++: internal compiler error: Killed (program cc1plus)
Please submit a full bug report,
with preprocessed source if appropriate.
See <http://bugzilla.redhat.com/bugzilla> for instructions.
make[2]: *** [sql/CMakeFiles/sql.dir/item_geofunc.cc.o] Error 4
make[1]: *** [sql/CMakeFiles/sql.dir/all] Error 2
make: *** [all] Error 2

[root@VM_77_151_centos mysql-5.7.17]# make
[100%] Built target my_safe_process
[root@VM_77_151_centos mysql-5.7.17]# make install
# 创建 mysql 用户和 mysql 用户组
[root@VM_77_151_centos ~]# groupadd -r mysql && useradd -r -g mysql -s /bin/false -M mysql
# 创建数据库存放目录，以及权限修改
[root@VM_77_151_centos mysql]# mkdir -p /usr/local/mysql/data && chown -R root:mysql /usr/local/mysql/
[root@VM_77_151_centos mysql]# chown -R mysql:mysql /usr/local/mysql/data/
[root@VM_77_151_centos mysql]# chmod -R go-rwx /usr/local/mysql/data/
# 初始化 MySQL 数据库
[root@VM_77_151_centos bin]# /usr/local/mysql/bin/mysqld --initialize-insecure --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data
# 启动
[root@VM_77_151_centos bin]# /usr/local/mysql/support-files/mysql.server start
[root@VM_77_151_centos lib]# echo "/usr/local/mysql/lib" > /etc/ld.so.conf.d/mysql.conf
```

`swap分区：http://blog.csdn.net/tongyijia/article/details/50783083`
`参考自:http://www.imooc.com/learn/703`
