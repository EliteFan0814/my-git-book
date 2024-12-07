### express 常用中间件

- express.json() 处理 Content-Type 为：application/json 的请求体
- express.raw() 处理 Content-Type 为：application/octet-stream 的请求体
- express.text() 处理 Content-Type 为：text/plain 的请求体
- express.urlencoded() 处理 Content-Type 为：application/x-www-form-urlencoded
  的请求体

**express 官方中间件竟然没有处理 Content-Type 为：multipart/form-data 的中间件！
！！**  
**express 官方中间件竟然没有处理 Content-Type 为：multipart/form-data 的中间件！
！！**  
**express 官方中间件竟然没有处理 Content-Type 为：multipart/form-data 的中间件！
！！**

可以使用 multiparty connect-multiparty 等第三方模块解决

- express.static()
- express.Router()
