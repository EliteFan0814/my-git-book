## form 表单中的 method 和 enctype 属性

1. 当 method="get" 时，enctype 属性无效，数据将以 URL 的查询字符串发出

   ```bash
   GET /handling-page?user_name=张三&user_passwd=123&submit_button=提交
   Host: example.com
   ```

2. 当 method="post" 时，enctype 属性默认为 'application/ x-www-form-urlencoded'，所有键值对会连接成一行，作为 HTTP 请求
   的数据体发送到服务器

   ```bash
   POST /handling-page HTTP/1.1
   Host: example.com
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 74

   user_name=张三&user_passwd=123&submit_button=提交
   ```

3. 当 method="post"，enctype = 'text/plain' 时，数据将以纯文本格式发送
4. 当 method="post"，enctype = 'multipart/form-data'，数据将以混合的格式发送，**这种格式也是文件上传的格式**

   ```bash
   Content-Type: multipart/form-data; boundary=---------------------------314911788813839

   -----------------------------314911788813839
   Content-Disposition: form-data; name="foo"

   bar
   -----------------------------314911788813839
   Content-Disposition: form-data; name="baz"

   The first line.
   The second line.

   -----------------------------314911788813839--
   ```

## 文件上传

本质就是：通过文件输入框选择本地文件，提交表单的时候，浏览器就会把这个文件发送到服务器。  
需要将表单 form 元素的 method 属性设为 POST，enctype 属性设为 multipart/form-data

例子（使用自定义脚本上传）：

```html
<div>
  <label for="up-img" class="label-style">上传图片</label>
  <input id="up-img" type="file" accept="image/*" style="opacity:0;width:0;henght:0;" />
</div>
<script>
  const input = document.getElementById('up-img')
  input.addEventListener('change', function (e) {
    console.log(e.target.files) // 等价于 console.log(input.value)，返回的是一个 FileList 对象
    const upInfo = e.target.files[0]
    const upForm = new FormData()
    upForm.append('file', upInfo)
    // 进行上传操作
    // request.post('xxx/xx/xx',upForm)
  })
</script>
```

**补充：** 文件选择器`<input type="file">`，出于安全考虑，浏览器不允许脚本自行设置这个控件的 value 属性，即文件必须是用
户手动选取的，不能是脚本指定的。一旦用户选好了文件，脚本就可以读取这个文件，返回一个 FileList 对象，每个成员都是一个 File 实例对象。File 实例对象是一个特殊的 Blob 实例，增加了name和lastModifiedDate属性。