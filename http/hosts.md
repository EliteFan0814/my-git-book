## windows 本地 hosts 文件指定域名和端口号

有时候本地开发时，需要不同的项目同时绑定 localhost  
想要使用 **localhost+不同端口号**来实现的话，可以使用如下方法：  
以下在 hosts 文件内配置：

```bash
# ····
# localhost name resolution is handled within DNS itself.
#	127.0.0.1       localhost
#	::1             localhost

192.168.10.153 windows10.microdone.cn

# 本地项目1
127.0.0.1 toocool.com
127.0.0.1 toocool.com:3000

# 本地项目2
127.0.0.1 avatar.com
127.0.0.1 avatar.com:3001
```

这样配置好后，使用:  
127.0.0.1:3000  
localhost:3000  
toocool.com:3000  
访问的项目就是绑定本地 3000 端口的项目。

127.0.0.1:3000  
localhost:3000  
avatar.com:3000  
访问的项目就是绑定本地 3001 端口的项目。

这样就可以同时使用 localhost 的不同端口绑定不同项目了。
