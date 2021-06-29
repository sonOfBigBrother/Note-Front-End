## 跨域资源共享（CORS)

跨域资源共享(Cross-Origin Resource Sharing) 是一种机制，它使用额外的HTTP头来告诉浏览器让运行在一个origin(domain) 上的Web应用被准许访问来自不同源服务器上的指定的资源。当一个资源从与该资源本身所在的服务器不同的域、协议或端口请求一个资源时，资源会发起一个跨域HTTP请求。  
出于安全原因，浏览器限制从脚本内发起的跨源HTTP请求。 例如，XMLHttpRequest和Fetch API遵循同源策略。 这意味着使用这些API的Web应用程序只能从加载应用程序的同一个域请求HTTP资源，除非响应报文包含了正确CORS响应头。  
需要应用CORS的场景：

- XMLHTTPRequest或Fetch发起的HTTP请求
- Web字体（CSS 中通过 @font-face 使用跨域字体资源）
- WebGL贴图
- 使用drawImage将Images/video画面绘制到canvas
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