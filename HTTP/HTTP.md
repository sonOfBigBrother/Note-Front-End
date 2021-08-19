[toc]

## 跨域资源共享（CORS)

​		跨域资源共享(Cross-Origin Resource Sharing) 是一种机制，它使用额外的HTTP头来告诉浏览器让运行在一个origin(domain) 上的Web应用被准许访问来自不同源服务器上的指定的资源。当一个资源从与该资源本身所在的服务器不同的域、协议或端口请求一个资源时，资源会发起一个跨域HTTP请求。  

​		出于安全原因，浏览器限制从脚本内发起的跨源HTTP请求。 例如，XMLHttpRequest和Fetch API遵循同源策略。 这意味着使用这些API的Web应用程序只能从加载应用程序的同一个域请求HTTP资源，除非响应报文包含了正确CORS响应头。  

​		需要应用CORS的场景：

- XMLHttpRequest或Fetch发起的HTTP请求
- Web字体（CSS 中通过 @font-face 使用跨域字体资源）
- WebGL贴图
- 使用drawImage将images/video画面绘制到canvas
- 样式表

简单请求不会触发CORS预请求，如下几个满足简单请求的条件：

- 使用如下方法之一：
  - GET
  - HEAD
  - POST方法
- Fetch规范定义了对CORS安全的首部字段集合，不得人为设置该集合之外的其他首部字段。集合如下：
  - Accept
  - Accept-Language
  - Content-Language
  - Content-Type（需要注意额外的限制）
  - DPR
  - Downlink
  - Save-Data
  - Viewport-Width
  - Width  
- Content-Type 的值仅限于下列三者之一：
  - text/plain
  - multipart/form-data
  - application/x-www-form-urlencoded
- 请求中的任意XMLHttpRequestUpload对象均没有注册任何事件监听器
- 请求中没有使用ReadableStream对象

## HTTP协议的特点与缺点

### 特点

- 无连接：每一次请求都要连接一次，请求结束就会断掉，不会保持连接。
- 无状态：每一次请求都是独立的，请求结束不会记录连接的任何信息，减少网络开销。
- 灵活：通过配置http协议中头部的`Content-Type`标记，可以传输任意数据类型的数据对象(文本、图片、视频等等)，非常灵活。
- 简单快速：发送请求访问某个资源时，只需传送请求方法和URL就可以了，使用简单。http协议简单，使得http服务器的程序规模小，因而通信速度很快

### 缺点

- 无状态：请求不会记录任何连接信息，没有记忆，就无法区分多个请求发起者身份是不是同一个客户端的，意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。
- 不安全：报文(header部分)使用的是明文——***明文传输***可能被窃听不安全；缺少***身份认证***，也可能遭遇伪装；还有缺少***报文完整性验证***，可能遭到篡改。
- 队头阻塞：开启**长连接**时，只建立一个TCP连接，同一时刻只能处理一个请求，那么当请求耗时过长时，其他请求就只能阻塞状态

### HTTP报文

- 请求报文
  - 请求行
  - 请求头
  - 空行
  - 请求体
- 响应报文
  - 状态行
  - 响应头
  - 空行
  - 响应体

### 参考资料

【1】[掘金文章](https://juejin.cn/post/6994629873985650696)

