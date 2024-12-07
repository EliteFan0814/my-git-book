---
description: docker学习
---

## 快捷键

- 查看 latest 镜像具体版本：`docker image inspect 镜像名称:latest | grep -i version`
- 删除镜像：`docker rmi 镜像ID1 镜像ID2`
- 删除容器：`docker rm 容器ID`
- 查看已在运行的容器 `docker ps -a`
- 在运行的容器中执行命令 `docker exec -it 容器名 命令`
  - `docker exec -it xxx /bin/bash`
  - `docker exec -it xxx /bin/sh /root/xxx.sh`
  - `docker exec -it 9df70f9a0714 /bin/bash`
- 启动 docker `systemctl start docker`
- 停止 docker `systemctl stop docker`
- 重启 docker `systemctl restart docker`
- 开机启动 docker `systemctl enable docker`
- 查看日志 `docker logs [容器名 | 容器ID ]`
  - `docker logs -f [容器名 | 容器ID ] #表示实时的跟踪日志输出`
  - `docker logs --since 30m myredis # 此处 --since 30m 是查看此容器30分钟之内的日志情况。`
- 复制容器内文件到本地 `docker cp 容器ID:目标路径 本地文件路径`

### windows 安装 redis 注意事项

windows 下使用 gitbash 安装 redis，例如这样：

```bash
docker run -d \
-p 6379:6379 \
--name ry-redis \
-v c:/home/redis/config/redis.conf:/etc/redis/redis.conf \
-v c:/home/redis/data:/data \
--log-opt max-size=100m --log-opt max-file=3 \
redis redis-server /etc/redis/redis.conf
```

出现安装完 redis 容器就报错停止运行，查看容器 logs：

```bash
$ docker logs redis容器名
1:C 27 Jan 2024 23:58:18.606 # Fatal error, can't open config file '/data/C:/Program Files/Git/etc/redis/redis.conf': No such file or directory

```

解决方法很简答，使用 windows 自带的终端或者 powershell 运行命令就行了

```bash
# linux中
docker run -d \
-p 6379:6379 \
--name ry-redis \
-v c:/home/redis/config/redis.conf:/etc/redis/redis.conf \
-v c:/home/redis/data:/data \
--log-opt max-size=100m --log-opt max-file=3 \
redis redis-server /etc/redis/redis.conf

# windows中使用 windows 自带的终端或者 powershell 运行：
docker run -d -p 6379:6379 --name ry-redis -v c:/home/redis/config/redis.conf:/etc/redis/redis.conf -v c:/home/redis/data:/data --log-opt max-size=100m --log-opt max-file=3 redis redis-server /etc/redis/redis.conf
```

## Docker 容器没有 vim 命令

```bash
apt-get update
apt-get install vim
```

## Docker mysql:5.7 时区问题最简单方法

启动时加`-e TZ=Asia/Shanghai`

```bash
docker run -d \
-p 3306:3306 \
--name ry-sql \
-v c:/home/mysql/log:/var/log/mysql \
-v c:/home/mysql/my.cnf:/etc/my.cnf \
-v c:/home/mysql/data:/var/lib/mysql \
-e TZ=Asia/Shanghai \
-e MYSQL_ROOT_PASSWORD=Woshishuaibi1314. \
--restart=always \
mysql:5.7.44
```

## docker 容器内访问宿主机 127.0.0.1 localhost 服务

docker 的 container 内，本身是一个微小的主机，那么在容器内请求 127.0.0.1 或者 localhost，自然是请求到了 container 本身的
网络，而无法抵达宿主机。例如，我用 docker 启动了一个 springboot 项目，项目中有这样一个配置文件：

```yml
# yml 配置文件
# ………………省略其它………………
  druid:
      # 主库数据源
      master:
               url: jdbc:mysql://localhost:3306/ry-vue?&zeroDateTimeBehavior=convertToNull&useSSL=true&serverTimezone=GMT%2B8
          username: root
          password: 123456
# ………………省略其它………………

```

然后我们创建镜像文件并新建容器：

```shell
docker build -t 我的镜像名 .

docker run -d \
--name 我的容器名 \
-p 8080:8080 \
-v c:/home/ruoyi/logs:/home/ruoyi/logs \
-v c:/home/ruoyi/uploadPath:/home/ruoyi/uploadPath \
-e TZ=Asia/Shanghai \
--restart=always \
我的镜像名
```

发现报错，提示连接不上数据库，因为我们需要连接到宿主机的 3306 端口的 mysql 数据库上，  
直接这样配置请求到的时容器内的 3306 端口，肯定报错。  
Docker 版本高于 v20.10 时引入了`host.docker.internal:host-gateway`参数  
我们修改配置如下：

```yml
# yml 配置文件
# ………………省略其它………………
  druid:
      # 主库数据源
      master:
               url: jdbc:mysql://host.docker.internal:3306/ry-vue?&zeroDateTimeBehavior=convertToNull&useSSL=true&serverTimezone=GMT%2B8
          username: root
          password: 123456
# ………………省略其它………………

```

然后重新创建镜像文件并新建容器，注意配置添加了一行：  
--add-host=host.docker.internal:host-gateway

```shell
docker build -t 我的镜像名 .

docker run -d \
--name 我的容器名 \
-p 8080:8080 \
-v c:/home/ruoyi/logs:/home/ruoyi/logs \
-v c:/home/ruoyi/uploadPath:/home/ruoyi/uploadPath \
-e TZ=Asia/Shanghai \
--add-host=host.docker.internal:host-gateway \
--restart=always \
我的镜像名
```

这样在容器内部的 jdbc 连接就能访问宿主机的端口了
