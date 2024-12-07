## 当前页点击下载

有时候后台管理需要导出功能，a 标签默认会打开新页面，此方法使用 dom 操作在当前页
面创建下载链接并点击下载

```javascript
 //下载任务文件事件 url 为下载地址
  function downloadFiles(url, name) {
    const aLink = document.createElement('a') //创建a链接
    aLink.style.display = 'none'
    aLink.href = url
    aLink.download = name
    document.body.appendChild(aLink)
    aLink.click()
    document.body.removeChild(aLink) //点击完成后记得删除创建的链接
  },
 //当后台返回的不是下载地址，而是文件的 blob 格式对象时
 //注意 blobObj 为 blob 格式对象！！！
 //  方法1：
 function downloadFiles(blobObj,fileName){
   const tempBlob = URL.createObjectURL(blobObj)
   const aLink = document.createElement('a')
   aLink.href = tempBlob
   aLink.download = fileName
   aLink.click()
   URL.revokeObjectURL(tempBlob)
 }
//  方法2：
 function downloadFiles(blobObj, fileName) {
   const reader = new FileReader()
   reader.readAsDataURL(blobObj)
   reader.onload = (e) => {
     const aLink = document.createElement('a')
     aLink.style.display = 'none'
     aLink.href = e.target.result
     aLink.download = 'fileName'
     document.body.appendChild(aLink)
     aLink.click()
     document.body.removeChild(aLink) //点击完成后记得删除创建的链接
   }
 }
```

### 举例，下载 application/vnd.ms-excel 格式文件

当请求返回的是 application/vnd.ms-excel 类型的数据时，<font color="red">发送
XMLHttpRequest 请求时，注意设置 responseType 为 blob</font>

```javascript
// 1 使用原生 XMLHttpRequest 发送请求
const xhr = new XMLHttpRequest();
xhr.open("GET", "/path/to/excel/template", true);
// 设置responseType属性的值，告诉浏览器如何解读返回的数据
xhr.responseType = "blob";
// load 事件表示服务器传来的数据接收完毕
xhr.onload = function (e) {
  if (this.status === 200) {
    // 如果返回的blob数据类型是 application/vnd.ms-excel
    if (xhr.response.type.includes("application/vnd.ms-excel")) {
      const blob = new Blob([xhr.response], {
        type: "application/vnd.ms-excel;charset=UTF-8",
      });
      const tempBlob = URL.createObjectURL(blob);
      const aLink = document.createElement("a");
      aLink.href = tempBlob;
      aLink.download = "模板文件";
      aLink.click();
      URL.revokeObjectURL(tempBlob);
    } else if (xhr.response.type.includes("application/json")) {
      // 如果返回的blob 数据类型是 application/json 则代表下载有问题,我们显示一下信息
      const reader = new FileReader();
      reader.readAsText(this.response, "utf-8");
      reader.onload = () => {
        const res = JSON.parse(reader.result);
        alert(res.msg);
      };
    }
  }
};
xhr.send();

// 2 使用 axios 发送请求
axios({
  method: "get",
  url: "/path/to/excel/template",
  responseType: "blob",
}).then((res) => {
  const blob = new Blob([res.data], {
    type: "application/vnd.ms-excel;charset=UTF-8",
  });
  const tempBlob = URL.createObjectURL(blob);
  const aLink = document.createElement("a");
  aLink.href = tempBlob;
  aLink.download = "模板文件";
  aLink.click();
  URL.revokeObjectURL(tempBlob);
});
```
