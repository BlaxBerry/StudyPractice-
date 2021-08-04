# PostgreSQL

PostgreSQL，是 关系数据库服务器（**OR**DBMS）的一种

> リレーショナル型データベース





## 安装使用

### Homebrew

下载最新版本：

```bash
brew install postgresql
```

下载指定版本：

```bash
 brew install postgresql@10
```

检查版本：

```bash
psql --version

psql (PostgreSQL) 13.3
```

检查PostgreSQL位置：

```bash
cd /usr/local/var
ls

homebrew	log		mongodb		mysql		postgres
```

卸载PostgreSQL：

```bash
brew uninstall postgresql
```

彻底删除PostgreSQL的初始化文件：

```bash
 rm -rf /usr/local/var/postgres
```



### App图形化安装 + 连接

1. 官网下载安装包

https://postgresapp.com/downloads.html

2. 打开后 initialize初始化

   就生成三个数据库：

   - 用户名（初始化时创建）
   - postgress（默认数据库）
   - template（模版）

   > **用户名：**
   >
   > 会在初始化时根据用户名创建

   > **postgress：**默认数据库
   >
   > 不存在其他数据库时，默认连接登陆该数据库

   > **template：**（数据库模版）
   >
   > 每次创建数据库时PostgreSQL都自动会参照template1的设置模版创建

3. 点击start开启PostgreSQL服务











## 视图化APP使用

### 登陆数据库

视图APP登陆数据库界面中，双击数据库，

会自动打开命令行工具，连接该数据库

```bash
Last login: Tue Aug  3 11:16:27 on ttys000
/Applications/Postgres.app/Contents/Versions/13/bin/psql -p5432 "chen"
chen@MacBook-Pro ~ % /Applications/Postgres.app/Contents/Versions/13/bin/psql -p5432 "chen"
psql (13.3)
Type "help" for help.

chen=# 
```

PostgreSQL默认连接端口是5432，简写为-p 432

登陆数据库一般需要密码

但是登陆本地数据库一般用来测试和开发，不需要密码



### 显示所有数据库

```bash
\l
```

```bash
chen=# \l
                                  List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges   
-----------+----------+----------+-------------+-------------+-----------------------
 chen      | chen     | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 template0 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
(4 rows)
```

在PostgreSQL的视图APP中，只显示了3个数据库

但是这里显示4个数据库，其中的 template0是 template1 的模版，

 template0不允许修改和登录，是因为 template1的设置可以被修改的，

万一出了问题，可通过 template0来恢复初始化





### 切换数据库

方法1：PostgreSQL视图化APP中双击数据库

---

方法2：命令行工具中

```bash
\c
```

```bash
chen=# \c postgres 
You are now connected to database "postgres" as user "chen".
postgres=# 
```

---

方法3：psql命令

```bash
\q
/Applications/Postgres.app/Contents/Versions/13/bin/psql -p5432 "数据库"
```

```bash
\q
/Applications/Postgres.app/Contents/Versions/13/bin/psql -p5432 "chen"
```





语句必须大写，且用分号结束



### 创建数据库

**CREATE DATABASE ?;**

```bash
chen=# CREATE DATABASE demo01;

chen=# \l

                                  List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges   
-----------+----------+----------+-------------+-------------+-----------------------
 chen      | chen     | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 demo01    | chen     | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 template0 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
(5 rows)
chen=# 
```





### 删除数据库

**DROP DATABASE ?;** 

```bash
chen=# DROP DATABASE demo01;
DROP DATABASE

chen=# \l
                                  List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges   
-----------+----------+----------+-------------+-------------+-----------------------
 chen      | chen     | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 template0 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
(4 rows)

(END)
```







# 





## 开启数据库服务

```bash
brew services start postgresql
```

连接数据库之前需要先开启数据库，

不然直接连接会报错，如下：

```bash
psql: could not connect to server: No such file or directory
Is the server running locally and accepting
connections on Unix domain socket “/tmp/.s.PGSQL.5432”?
```



## 连接数据库

开启数据库服务后，输入：

```bash
psql postgres
```

然后终端表示会变为如下：

```bash
postgres=#
```



PostgreSQLとの接続を終了

```bash
postgres=# \q
```

