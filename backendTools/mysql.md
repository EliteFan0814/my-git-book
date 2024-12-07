---
description: mysql常见问题
---

## sql 常用语句

- SHOW CREATE TABLE employees; 展示表创建时的参数
- DESCRIBE employees; 展示表结构
- SELECT xxx <=> null from tableName; 查询表中某列值是否为 null
- SELECT xxx FROM table1 LIMIT (pageNumber - 1) \* pageSize, pageSize; 分页查询

## 小知识

- 如果有 n 个表实现多表查询，则需要至少 n-1 个连接条件
- UNION ALL 操作符返回两个查询结果集的并集，对于结果集中重复的部分不予处理，效率比 UNION 高

## sql 语句执行顺序

列的别名只能在 ORDER BY 中使用，不能在 WHERE 中使用。

```sql
SELECT name1,name2,name3 AS N3
FROM table1
WHERE name1 IN ('a','b','c')
ORDER BY N3 DESC;
```

因为查询语句是从`FROME xxx WHERE xxx`开始的，然后才是`SELECT` `ORDER BY` 。  
`WHERE`也需要声明在`FROM`后，`ORDER BY`之前。

## navicat 连接 mysql8 报错：1251

navicat 连接 mysql8.1.0 报错，错误信息：1251- Client does not support authentication protocol requested by
server;consider upgrading Mysql client。

出现这种情况的原因是：MySQL8 之前的版本中加密规则是 mysql_native_password,而在 MySQL8 之后,加密规则是
caching_sha2_password

```bash
mysql> select host,user,plugin,authentication_string from mysql.user;
```

![user.png](https://s2.loli.net/2024/06/14/ubEySP82jQIqwpf.png)

### 解决方法

```bash
mysql> ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
Query OK, 0 rows affected (0.01 sec)

mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
Query OK, 0 rows affected (0.01 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.01 sec)

mysql> select host,user,plugin,authentication_string from mysql.user;
```

![user2.png](https://s2.loli.net/2024/06/14/o21MUXNSgs8TqGW.png)

再用 navicat 连接就成功了

ps:

> mysql.user 表中 Host 列的含义： Host 列指定了允许用户登录所使用的 IP，  
> 比如 user=root Host=192.168.1.1。  
> 这里的意思就 是说 root 用户只能通过 192.168.1.1 的客户端去访问。  
> 而%是个通配符，如果 Host=192.168.1.%，那么就表示只要是 IP 地址前缀为“192.168.1.”的客户端都可以连接。如果 Host=%，表示
> 所有 IP 都有连接权限。

> mysql 配置文件 !includedir /etc/mysql/conf.d/ 含义:  
> 在 MySQL 中，!includedir 是一个指令，用于指定一个目录，MySQL 服务器在读取主配置文件时会读取该目录下所有的.conf 文件
> 。/etc/mysql/conf.d/ 是一个目录路径，MySQL 服务器会尝试读取这个目录下所有的.conf 文件作为配置文件的一部分。
