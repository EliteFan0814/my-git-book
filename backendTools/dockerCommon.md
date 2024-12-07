---
description: docker常见问题
---

## docker 安装 mysql 8

```bash
# 本地主机创建数据映射的目录
mkdir -p c:/home/mysql-test/conf/
mkdir -p c:/home/mysql-test/data/
mkdir -p c:/home/mysql-test/log/

# 下载镜像
docker pull mysql:8.0.17
# 运行mysql命名，容器名称为mysql8并且设置root账号初始密码
docker run  -p 3306:3306 --name mysql8 -e MYSQL_ROOT_PASSWORD="123456" -d mysql:8.0.17
#进入docker容器，mysql8为刚安装的容器名称
docker exec -it mysql8 bash
# 在Docker内，复制MySQL自定义配置文件到本地主机
docker cp mysql8:/etc/mysql/conf.d/mysql.cnf c:/home/mysql-test/conf/mysql.cnf
# 修改字符集
vim c:/home/mysql-test/conf/mysql.cnf
character-set-server=utf8
# 停止并删除容器mysql8
docker stop mysql8 && docker rm mysql8
# 重新创建容器
docker run -d \
-p 3308:3306 \
--name test-sql \
-v c:/home/mysql-test/conf/mysql.cnf:/etc/mysql/conf.d/mysql.cnf \
-v c:/home/mysql-test/log:/logs \
-v c:/home/mysql-test/data:/var/lib/mysql \
-e TZ=Asia/Shanghai \
-e MYSQL_ROOT_PASSWORD=123456 \
--restart=always \
mysql:8.0.17
```
