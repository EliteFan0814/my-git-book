服务器向浏览器推送信息，除了 WebSocket，还有一种方法：Server-Sent Events（SSE）。  
严格地说，HTTP 协议无法做到服务器主动推送信息。  
但是，有一种变通方法，就是服务器向客户端声明，接下来要发送的是流信息（streaming）。

也就是说，发送的不是一次性的数据包，而是一个数据流，会连续不断地发送过来。这时，客户端不会关闭连接，会一直等着服务器发过
来的新的数据流，视频播放就是这样的例子。本质上，这种通信就是以流信息的方式，完成一次用时很长的下载。

SSE 就是利用这种机制，使用流信息向浏览器推送信息。它基于 HTTP 协议。  
SSE 是单向通道，只能服务器向浏览器发送，因为流信息本质上就是下载。

SSE 和 webSocket 的区别

- SSE 使用 HTTP 协议，现有的服务器软件都支持。WebSocket 是一个独立协议。
- SSE 属于轻量级，使用简单；WebSocket 协议相对复杂。
- SSE 默认支持断线重连，WebSocket 需要自己实现。
- SSE 一般只用来传送文本，二进制数据需要编码后传送，WebSocket 默认支持传送二进制数据。
- SSE 支持自定义发送的消息类型。

SSE 的客户端 API 部署在 EventSource 对象上。具体教程可
看[Server-Sent Events 教程](https://www.ruanyifeng.com/blog/2017/05/server-sent_events.html) 。

这里我们使用 fetch api 处理 Content-Type 为 text/event-stream 的流数据

定义类：

```ts
// requestFetch.ts

export class FetchAiStream {
  controllerAbort: AbortController;
  constructor() {
    this.controllerAbort = new AbortController();
  }
  fetchStream(url: string, params: any) {
    const { onmessage, onabort, onclose, headers, ...otherParams } = params;
    // 错误处理程序
    const errorHandler = (err: any) => {
      if (err.message.includes("aborted")) {
        onabort(err);
      } else {
        console.log(`错误信息：${err.message ?? "未知错误"}`);
      }
    };
    const push = async (controller: any, reader: ReadableStreamDefaultReader) => {
      try {
        const { value, done } = await reader.read();
        if (done) {
          controller.close();
          onclose?.();
        } else {
          onmessage?.(new TextDecoder().decode(value));
          controller.enqueue(value);
          push(controller, reader);
        }
      } catch (err: any) {
        // controller.console;
        errorHandler(err);
      }
    };
    // 发送请求
    return fetch(url, {
      signal: this.controllerAbort.signal,
      headers: { "Content-Type": "application/json", ...headers },
      ...otherParams,
    })
      .then((response: Response) => {
        // 以ReadableStream解析数据
        const reader = response.body?.getReader();
        const stream = new ReadableStream({
          start(controller) {
            push(controller, reader);
          },
        });
        return stream;
      })
      .then((stream) => new Response(stream, { headers: { "Content-Type": "text/html" } }).text())
      .catch((err: Error) => {
        errorHandler(err);
      });
  }
  abort() {
    this.controllerAbort.abort();
  }
}
```

使用：

```ts
import { FetchAiStream } from "./requestFetch";
// 构造自定义fetch请求的实例
let fetchAiAnswer: FetchAiStream;
fetchAiAnswer = new FetchAiStream();

fetchAiAnswer.fetchStream("https://chatgpt.cmm/xxx/xx/x", {
  method: "post",
  body: JSON.stringify({
    message: "帮我随机生成一个表格",
  }),
  headers: {
    accept: "text/event-stream",
  },
  // 处理获取的数据流
  onmessage: (res: any) => {
    console.log(JSON.stringify(res));
  },
  // 中断请求
  onabort: (res: any) => {
    console.log("中断请求成功");
  },
  // 请求完成
  onclose: (res: any) => {
    console.log("请求完成");
  },
});
```
