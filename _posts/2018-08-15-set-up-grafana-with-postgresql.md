---
layout: post
title: 搭建以PostgreSQL为查询DB的Grafana
comments: true
category: misc
permalink: /:year/:month/:day/:title
tag: misc, tech
---

### 搭建Graphana
想搭建一个简单的图表平台能每天查询新增用户等数据，刚开始想用[Redash](https://redash.io)，奈何docker的版本总是报一个关于python的错误，查了一下是关于linux 16.04和python兼容问题，以为需要快速搭建，于是尝试改用了[Grafana](https://grafana.com/)。

首先下载安装包，非常方便的是，它直接提供了.deb，一键安装无忧。安装完成修改配置文件`/etc/grafana/grafana.ini`，这个文件里保存着默认的admin密码、登录方式、端口号等。因为默认3000端口已经被其他进程占用，所以修改`http_port = 3001
`。保存然后启动：

```
sudo service grafana-server start
```

打开链接http://0.0.0.0:3001，使用默认密码登录(用户名：admin，密码：admin)。然后添加数据库Postgresql。

### 设置数据库
像这种只为了分析数据而做的数据库，最好在DB上建立一个只读的user，因为这种分析平台是不会去检测query的正确性，如果一不小心drop了某个表就会很悲剧了。

```
-- Create a group
CREATE ROLE readaccess;

-- Grant access to existing tables
GRANT USAGE ON SCHEMA public TO readaccess;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO readaccess;

-- Grant access to future tables
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO readaccess;

-- Create a final user with password
CREATE USER read_only WITH PASSWORD 'dowhateveryouwant';
GRANT readaccess TO  read_only;

```
如果是要用grafana监听本地的postgresql，到这一步就可以在grafana上用用户`read_only`，密码`dowhateveryouwant`直接连接数据库了。
如果要用远程的DB，还需要打开DB的连接权限。
查看config文件地点
```
sudo -u postgres psql
postgres=# show config_file;
               config_file                
------------------------------------------
 /etc/postgresql/9.5/main/postgresql.conf
(1 row)

```
打开config文件`/etc/postgresql/9.5/main/postgresql.conf`修改

```
listen_addresses = '*'
```
在config文件里找到pg_hba.config（Host Based Authentication）文件的地址`hba_file = '/etc/postgresql/9.5/main/pg_hba.conf'`
修改`/etc/postgresql/9.5/main/pg_hba.conf`，在最后添加
```
host    my_database     read_only        10.10.64.108/32 trust
```
10.10.64.108是允许访问的IP地址，32是默认的TCP端口，trust表示无需认证。
然后重启postgresql，就可以添加访问了postgresql了

添加完DB后就能愉快的在grafana里添加想要监控的图表了。

