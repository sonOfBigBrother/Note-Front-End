## 问题汇总
### 本地文件调用引起的跨域访问错误
错误信息如下：        

> Access to script at 'file:///E:/Practice/Web/practice-for-front-end/rich-text-input/app.js' from origin 'null' has been blocked by CORS policy: Cross origin requests are only supported for protocol schemes: http, data, chrome, chrome-extension, https.   

解决方案：  
安装anywhere库运行本地文件，参看[github上的说明](https://github.com/JacksonTian/anywhere)。

### 定制input标签
个人觉得默认的input标签样式很丑，希望能够美化一下，在网络上找到了这篇blog [Styling & Customizing File Inputs the Smart Way](https://tympanus.net/codrops/2015/09/15/styling-customizing-file-inputs-smart-way/)
### 使用Vue官方脚手架搭建项目后运行npm run serve报错
错误信息如下：

> Module parse failed: Unexpected token.  
> You may need an appropriate loader to handle this file type.

解决方案：
经查询，发现误将Vue的文件扩展名写成大写，脚手架中默认使用的vue-loader无法匹配文件，改成小写即可。  
[参考资料](https://github.com/symfony/webpack-encore/issues/321)