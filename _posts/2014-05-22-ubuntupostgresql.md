---
layout: post
title: "Ubuntu下Postgresql的使用"
categories: Database
tags: Ubunt, Postgresql
---


---
####一、在Ubuntu下安装Postgresql

    $ sudo apt-get install postgresql-9.1 postgresql-client-9.1   
                           postgresql-contrib-9.1

####二、修改PostgreSQL数据库的默认用户postgres的密码
######1、PostgreSQL登录(使用psql客户端登录)：

    $ sudo -u postgres psql
    # PostgreSQL数据默认会创建一个postgres的数据库用户作为数据库的管理员

######2、修改PostgreSQL登录密码：
    
    $ postgres=# ALTER USER postgres WITH PASSWORD 'postgres';
    # postgres=#为PostgreSQL下的命令提示符

######3、退出PostgreSQL psql客户端：

    $ postgres=# \q


####三、修改linux系统的postgres用户的密码

######1、删除PostgreSQL用户密码：

     $ sudo passwd -d postgres
     # passwd -d 是清空指定用户密码的意思

######2、设置PostgreSQL用户密码  
PostgreSQL数据默认会创建一个linux用户postgres，通过上面的代码修改密码        为'postgres’  
 
    $ sudo -u postgres passwd


####四：修改PostgresSQL数据库配置实现远程访问
#####1、修改postgresql.conf
    $ vi /etc/postgresql/9.1/main/postgresql.conf

 * 监听任何地址访问，修改连接权限  
    #listen_addresses = ‘localhost’改为 listen_addresses = ‘*’       
 
 * 启用密码验证  
    #password_encryption = on 改为 password_encryption = on

#####2、可访问的用户ip段  
    $ vi /etc/postgresql/9.1/main/pg_hba.conf

在文档末尾加上以下内容  
  # to allow your client visiting postgresql server  
  host all all 0.0.0.0 0.0.0.0 md5

#####3、重启PostgreSQL数据库      
    $ /etc/init.d/postgresql restart

        
         
####五、管理PostgreSQL用户和数据库
#####1、登录postgre SQL数据库           
    $ psql -U postgres -h 127.0.0.1

#####2.创建新用户zhaofeng，但不给建数据库的权限  

    $ postgres=# create user “zhaofeng” with password ‘123456’ nocreatedb
    # 注意用户名要用双引号，以区分大小写，密码不用

#####3、建立数据库，并指定所有者           
    $ postgres=# create database “testdb” with owner=”zhaofeng”;

#####4、在外部命令行的管理命令   
    $ -u postgres createuser -D -P test1  
    # -D该用户没有创建数据库的权利，-P提示输入密码,选择管理类型y/n 
    -u postgres createdb -O test1 db1
    /-O设定所有者为test1


####第六步、安装postgresql数据库pgAdmin3客户端管理程序
    $ sudo apt-get install pgadmin3