[toc]

---
## HTML语义化及其优点
语义化是指文本内容的结构化（内容语义化），选择合乎语义的标签（代码语义化），便于开发者在阅读，维护和写出更优雅的代码的同时，让浏览器的爬虫和辅助技术更好地解析。

语义化的优点：  

- 可访问性：帮助辅助技术更好地阅读和转译你的网页，利于无障碍阅读。
- 可检索性：有了良好的结构和语义，可以提高搜索引擎的有效爬取，提高网站流量。
- 可维护性：减少网页间的差异性，方便后期开发和维护。  

常见的语义化标签及其说明：

- &lt;header&gt;：文档或文章的头部信息，如标题、logo和搜索框
- &lt;nav&gt;：文章的导航栏、链接等；
- &lt;main&gt;：文章的主要内容，在一份文档中是<strong>唯一</strong>的,后代元素常包含&lt;article&gt;。	
- &lt;article&gt;：文档、页面、应用或网站中的独立结构，其意在成为可独立分配的或可复用的结构，如论坛帖子、新闻文章或博客等。
- &lt;section&gt;：表示整体的一部分或文章中的一节，一般来说包含一个标题。
- &lt;aside&gt;：一个和其余页面内容几乎无关的部分，被认为是独立于该内容的一部分并且可以被单独的拆分出来而不会使整体受影响，通常表现为侧边栏或者嵌入内容。
- &lt;footer&gt;：最近一个章节内容或者根节点元素的页脚，通常包含作者、版权信息或者相关链接

此处插入&lt;i&gt;和&lt;em&gt;、&lt;b&gt;和&lt;strong&gt;的比较来澄清语义化上的一些混淆：  

- &lt;i&gt;表示一段普通文本中，因为某种原因和正常文本不同，例如专业术语、外语短语或排版用的文字，其通常表现为斜体，不会改变语义；而&lt;em&gt;表示强调文本内容，强调位置的不同会改变语义，其表现形式也是斜体。
- &lt;b&gt;中的文本通常显示粗体，用于吸引读者的注意力，不具备强调重要性的作用，也不影响语态和语气；&lt;strong&gt;强调内容的重要性、严重性或紧急性。  

具体细节可以查看<a href="http://w3c.github.io/html/textlevel-semantics.html#the-em-element">W3C的标签规范文档</a>
## 置换元素
一个内容不受CSS视觉格式化模型控制，CSS渲染模型并不考虑对此内容的渲染，且本身一般拥有固定尺寸的元素。
## 行内元素、块级元素与行内块元素比较
### 行内元素
特性：

- 和相邻的行内元素都在一行上，遇到父级元素边界会自动换行。
- 忽略width和height设定值
- 宽度取决于内容
- margin和padding的设定在水平方向上有效，竖直方向上无效。翻阅<a href="https://drafts.csswg.org/css2/box.html#propdef-margin-top">CSS盒子模型的文档</a>可以知晓margin-top和margin-bottom对行内非置换元素无效
- 只能容纳其他行内元素或文本。<strong>Note:根据<a href="http://w3c.github.io/html/textlevel-semantics.html#the-a-element">W3C标签规范文档</a>，虽然&lt;a&gt;是行内元素，但它可以插入&lt;p&gt;、&lt;section&gt;和&lt;table&gt;等块级元素</strong>   

### 块级元素
特性：

- 独占一行
- 可以设置宽度、高度和内外边距
- 默认宽度为父元素宽度
- 可以容纳行内元素和其他块元素  

### 行内块元素
特性：

- 相邻行内元素在同一行，但是之间会有空白缝隙
- 宽度取决于内容
- 宽度、高度、行高、外边距以及内边距都可以设置

可以通过display属性切换这三种元素，如为行内元素设置display：block则变换为块元素。

## js脚本的引入
下面介绍几种引入JavaScript脚本的方式，讨论每种方式的特点和差异，此处不讨论内联式脚本，与正常HTML解析流程一致。
### &lt;head&gt;中引入
在head标签中引入脚本，不设置*defer*和*async*属性，当HTML解析器解析到script标签时，将会去获取js源码并执行代码，如下图所示： 
![](https://i.imgur.com/gfHFVUV.png)
### &lt;body&gt;末端引入
在body标签尾部中引入脚本，不设置*defer*和*async*属性，HTML解析不需要暂停，当解析完成后脚本加载完毕并执行，如下图所示：
![](https://i.imgur.com/lRLXSbc.png)
### 在head标签中引入且设置defer属性
当页面在head标签中引入设置*defer*属性的脚本时，HTML页面解析的示意图如下：
![](https://i.imgur.com/vqC4Pw2.png)
从图中可以看出，脚本时异步加载，当HTML解析完毕时执行。
### 在head标签中引入且设置async属性
当页面在head标签中引入设置*async*属性的脚本时，HTML页面解析的示意图如下：
![](https://i.imgur.com/EAZL3oC.png)
从图中可以看出，脚本也是异步加载，且加载完毕后阻塞HTML解析，执行脚本完成后恢复解析过程。

### async VS defer

- async方式加载的脚本会在下载完成后立刻解析并执行，而defer的脚本在HTML解析完成后执行。
- defer的脚本按照在HTML文档中出现的位置按序加载，而async可能乱序。
- 两者虽然不会阻塞HTML解析，但是可能会阻塞渲染。

由于async的脚本可能会阻塞同步脚本（即HTML中普通加载的JS脚本），而同步脚本对于页面中的关键内容很重要，所以在async和defer间优选择defer  
<b>参考资料</b>：  
[Prefer DEFER Over ASYNC](https://calendar.perfplanet.com/2016/prefer-defer-over-async/)  
[Efficiently load JavaScript with defer and async](https://flaviocopes.com/javascript-async-defer/)