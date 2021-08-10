[toc]

## 选择器的选择规则

### 重要性(Importance)
重要性按序号依次加强：  

- 用户代理样式表的声明 (例如：浏览器在没有其他声明时的默认样式).  
- 用户样式表中的普通声明（由用户设置的自定义样式）。  
- 作者样式表中的普通声明（Web开发人员设置）。  
- 作者样式表中的重要声明  
- 用户样式表中的重要声明  

<b>Note:</b>可以使用特殊语法提升重要性——*!important*

### 专用性(Specificity)
使用四位的数值衡量专用性： 

- 声明在style属性中千位为1，否则为0。  
- 整个选择器中每包含一个ID选择器，百位加1。  
- 选择器中每包含一个类选择器、属性选择器和伪类，十位加1。  
- 选择器中每包含一个元素选择器和伪元素，个位加1。  
- 四位数值较大的选择器，优先显示其特性。      

### 源代码次序(Source Order)

当多个选择器具有相同的重要性和选择性时，后定义的规则覆盖先定义的。

### 基于关系的选择器

| 选择器　|选择的元素|
|--------|---------|
|A   B|元素A的任一后代元素B|
|A > B|元素A的任一子元素B（直系子代）|
|A:first-child|任一父元素的首个子元素为A的元素A|
|A + B|元素A的任一下个兄弟元素B|
|A ~ B|元素A后拥有共同父元素的兄弟元素B|



## 继承
提供了四种特殊的通用属性值处理继承：

- <i>inherit</i>:选定元素的属性值设置为与其父元素一样  
- <i>initial</i>:选定元素的属性值设置为与<strong>浏览器默认样式表</strong>中该元素设置的值一样。如果浏览器认样式表中没有设置值，并且该属性是自然继承的，那么该属性值就被设置为 <i>inherit</i>。  
- <i>unset</i>:将属性重置为其自然值，即如果属性是自然继承的，那么它就表现得像<i>inherit</i>，否则就是表现得像<i>initial</i>。   
- <i>revert</i>:属性值被设置成<strong>自定义样式</strong>所定义的属性，否则属性值被设置成<strong>用户代理的默认样式</strong>。

## 盒模型(Box Model)

### 标准盒子模型定义

每个box由4个部分组成：内容（content）、内边距（padding）、边框（border）和外边距（margin）  
<img src="https://mdn.mozillademos.org/files/8685/boxmodel-(3).png" alt="CSS Box model" style="height: 384px; width: 548px;">  
<b><i>box-sizing</i></b>属性可以更改默认的CSS盒子模型，默认值为<i>content-box</i>，当前元素的宽度即content的宽度，若加上padding和border的宽度大于父元素宽度则会产生溢出问题。当设为<i>border-box</i>且设置该元素宽度时，content、padding、border的宽度之和为该宽度值，另规定content的最小宽度为0。  
<b><i>Note:</i></b>  
在Firefox中实现了padding-box，即width = content_width+padding_width;height = content_height+padding_height,但在<b>Firefox 50</b>中已被删除。  
box-sizing中没有margin-box设定值，margin本身并不改变元素盒子的尺寸，没有价值且使用场景有限，见<a href="https://www.zhangxinxu.com/wordpress/2016/09/talking-about-css-margin-box/">张旭鑫的文章</a> 

### 标准盒子模型和IE盒子模型的区别

盒子宽度的定义和计算方式不同，其中IE盒子的宽度计算种不包含padding和border——意味着在设置二者后内容的宽度会缩减。

- 标准盒子：盒子width = margin-left + border-left + padding-left + width + padding-right + border-right + margin-right
- IE盒子：盒子width = margin-left + width + margin-right

***Note***：IE 6及之后的版本运行在标准兼容模式下时使用标准盒子模型

[参考资料](https://www.456bereastreet.com/archive/200612/internet_explorer_and_the_css_box_model/)

## 块级格式化上下文(Block Formatting Context)
### 定义
Web页面可视化CSS渲染的一部分，布局过程中生成<strong>块级盒子</strong>的区域，也是浮动元素和其他元素的交互限定区。

### 原理

- BFC内部元素在垂直方向上的边距会发生重叠
- BFC不会与浮动元素重叠
- BFC是独立的容器，里外的元素互不影响
- 计算BFC元素高度时，浮动元素参与计算 

### 产生条件

- 根元素或包含根元素的元素  
- 浮动元素(float属性不为none)  
- 绝对定位元素(position:absolute/fixed) 
- overflow属性不为visible的块元素  
- 行内块元素(display:inline-block)  
- 表格单元格(display:	table-cell)  
- 表格标题(display:tabel-caption)  
- 匿名表格单元格元素(display:table/table-row/table-row-group/table-header-group/table-footer-group/inline-table)   
- display为flow-root的元素  
- contain为layout、content、strict的元素  
- 弹性元素(display:flex/inline-flex元素的直接子元素)  
- 网格元素(display:grid/inline-grid元素的直接子元素)  
- 多列容器(column-count/column-width不为auto)  
- column-span属性为all的元素

### 应用

- 阻止margin重叠(margin collapsing)  
	margin collapsing的三种基本情况：  
	
	- 相邻元素间（除非后一个元素需要清除之前的浮动）  
	- 父元素与第一个子元素或最后一个子元素：父子元素间不存在分开margin-top/margin-bottom的边框、内边距和内容等  
	
- 空的块级元素：不含任何内容，且margin-top和margin-bottom没有分隔	  
	<b><i>note:</i></b>  
		- 上述情况的组合会产生更复杂的外边距折叠。  
		- 即使某一外边距为0，这些规则仍然适用。因此就算父元素的外边距是0，第一个或最后一个子元素的外边距仍然会“溢出”到父元素的外面。  
		- 如果参与折叠的外边距中包含负值，折叠后的外边距的值为最大的正边距与最小的负边距（即绝对值最大的负边距）的和。  
		- 如果所有参与折叠的外边距都为负，折叠后的外边距的值为最小的负边距的值。这一规则适用于相邻元素和嵌套元素。 
	
- 清除浮动 

  ```html
  <div style="border: 1px solid #000;">
      <div style="width: 100px;height: 100px;background: #eee;float: left;"></div>;  
  </div>;
  ```

  ![](https://pic4.zhimg.com/80/v2-371eb702274af831df909b2c55d6a14b_hd.png)

  由于容器内元素浮动，脱离了文档流，容器只剩下2px的边框距。若触发容器BFC，则容器会包裹浮动元素。外层容器添加"overflow:hidden"即可。

  ![](https://pic4.zhimg.com/80/v2-cc8365db5c9cc5ca003ce9afe88592e7_hd.png)

- 阻止元素被浮动元素覆盖  
  下为文字环绕效果:    

  ```html
  <div style="height: 100px;width: 100px;float: left;background: lightblue">
      我是一个左浮动的元素</div>  
  <div style="width: 200px; height: 200px;background: #eee">我是一个没有设置浮动,也没有触发 BFC 元素, width: 200px; height:200px; background: #eee;</div>
  
  ```

  ![](https://pic4.zhimg.com/80/v2-dd3e636d73682140bf4a781bcd6f576b_hd.png)  
  第二个元素有部分被浮动元素所覆盖，(但是文本信息不会被浮动元素所覆盖) 如果想避免元素被覆盖，可触第二个元素的 BFC 特性，在第二个元素中加入 overflow: hidden，就会变成：
  ![](https://pic3.zhimg.com/80/v2-5ebd48f09fac875f0bd25823c76ba7fa_hd.png)

- 自适应两栏布局
见项目E:\Practice\Web\practice-for-front-end\two-column-layout

## 层叠上下文

在[CSS2规范](https://drafts.csswg.org/css2/visuren.html#propdef-z-index)中提到，每个盒模型的位置是三维的，分别为平面画布中的x轴、y轴以及表示层叠的z轴。对于每个html元素，都可以通过设置z-index属性来设置该元素在视觉渲染模型中的层叠顺序。  
z-index可以设置成三个值：

- auto，默认值。当设置为auto的时候，当前元素的层叠级数是0，同时这个盒不会创建新的层级上下文（除非是根元素）；
- &lt;integer&gt;指示层叠级数，无论是何整数值，都会创建一个新的层叠上下文；
- inherit。

除了由根元素创建的根层叠上下文以外，其它上下文是由z-index不为auto的<i>positioned</i>元素所创建。

### 特征

- 可以嵌套
- 每个层叠上下文相对于兄弟元素是完全独立的，其内部规则不会影响到外部	。
- 每个层叠上下文都会被父层叠上下文当作一个元素施加层叠规则。  

### 产生条件

- 根元素（&lt;html&gt;）
- <i>position:fixed</i>
- z-index值不为<i>auto</i>的绝对/相对定位
- flex项目且z-index不为<i>auto</i>
- transform不为<i>none</i>
- opacity < 1
- mix-blend-mode不为<i>normal</i>
- filter不为<i>none</i>
- perspective不为<i>none</i>
- isolation为<i>isolate</i>
- will-change
- webkit-overflow-scrolling为<i>touch</i>  

### 规则
绘制的规则按照先后次序绘制：

- 创建层叠上下文的元素的背景和边界；
- z-index为负值的子元素，数值越小越早被绘制；
- 同时满足<i>in-flow</i>、<i>non-inline-level</i>、<i>non-positioned</i>的后代元素；
- <i>non-positioned</i>的浮动元素；
- 满足<i>in-flow</i>、<i>inline-level</i>、<i>non-positioned</i>的后代元素，包含行内表格和块；
- 层叠级数为0的子层叠上下文以及<i>positioned</i>且层叠级数为0的后代元素；
- 层叠级数大于等于1的<i>positioned</i>子层叠上下文，数值越小越早被绘制； 

## link和@import的区别

- link可以定义rss和rel，而@import只能加载css
- 解析link标签时同步加载引用的css，而@import引用的css在页面加载完时加载
- link可以用js动态引入，而@import不行

## 居中布局
### 水平居中

- 若是行内元素，为其父元素设置<i>text-align:center</i>即可
- 若是块级元素，该元素设置<i>margin:0 auto</i>
- 子元素包含<i>float:left</i>属性，则可让父元素宽度设置为<i>fit-content</i>，并且配合margin。另应注意<b>fit-content</b>是CSS3中新增的属性值。
```css
.parent{
    width: -moz-fit-content;
    width: -webkit-fit-content;
    width:fit-content;
    margin:0 auto;
}
```
- 使用flex + justify-content:
```css
.son{
    display: flex;
    justify-content: center;
}
```
- 使用flex 2009年的语法，与前述2012年的标准flex语法不同，是需要使用前缀的：
```css
.parent {
    display: -webkit-box;
    -webkit-box-orient: horizontal;
    -webkit-box-pack: center;
    display: -moz-box;
    -moz-box-orient: horizontal;
    -moz-box-pack: center;
    display: -o-box;
    -o-box-orient: horizontal;
    -o-box-pack: center;
    display: -ms-box;
    -ms-box-orient: horizontal;
    -ms-box-pack: center;
    display: box;
    box-orient: horizontal;
    box-pack: center;
}
```
- CSS3中的新增属性<i>transform</i>：
```css
.son{
    position:absolute;
    left:50%;
    transform:translate(-50%,0);
}
```
### 垂直居中

- 若是单行文本，设置<i>line-height</i>等于父元素高度。
- 若是行内块级元素，使用<i>vertical-align</i>和伪元素实现：
```css
.parent::after, .son{
    display:inline-block;
    vertical-align:middle;
}
.parent::after{
    content:'';
    height:100%;
}
```
- 使用flex和align-item：
```css
.parent {
  display: flex;
  align-items: center;
}
```
- 父元素设置相对定位，子元素使用transform：
```css
.son{
    position:absolute;
    top:50%;
    -webkit-transform: translate(-50%,-50%);  
    -ms-transform: translate(-50%,-50%);
    transform: translate(-50%,-50%);
}
```
- 父元素设置相对定位，子元素使用：
```css
.son{
    position:absolute;
    height:固定值;
    top:0;
    bottom:0;
    margin:auto 0;
}
```

## visibility:hidden和display:none比较

### 深入display:none

为元素设置```display:none```后，界面不显示该元素，且不占布局空间，但仍可以通过JavaScript操作该元素。出现这种现象的原因是：浏览器会解析HTML标签生成DOM Tree，解析CSS生成CSSOM，然后将DOM Tree和CSSOM合成生成Render Tree，元素在Render Tree中对应0或多个盒子，然后浏览器以盒子模型的信息布局和渲染界面。设置```display:none```的元素在Render Tree中没有生成对应的盒子模型，因此后续的布局、渲染工作自然没它什么事了，至于DOM操作还是可以的。

#### 特点

- 存在默认设置```display:none```样式的元素，如link、script、style、dialog和input[type=hidden]。
- HTML5中新增hidden布尔属性，让开发者自定义元素的隐藏性
- 父元素设置为```display:none```时该属性会侵入到其下所有后代元素中。
- 无法获取焦点
- 无法响应任何事件，无论是捕获、命中目标和冒泡阶段均不可以。
- 不影响form表单提交数据
- CSS的counter属性会忽略```display:none```的元素

 ```html
 .start{
   counter-reset: son 0;
 }
 .son{
   counter-increment: son 1;
 }
 .son::before{
   content: counter(son) ". ";
 }
 
 <div class="start">
   <div class="son">son1</div>
   <div class="son" style="display:none">son2</div>
   <div class="son">son3</div>
 </div>
 <!-- result -->
 <!-- 1. son1-->
 <!-- 2. son3-->
 ```

- 使transition失效
- display变化时将触发reflow

### 深入visibility

#### 作用

- 用于隐藏表格的行和列
- 用于在不触发布局的情况下隐藏元素

#### 有效值

- visible
- hidden：元素不可见，但保留原来占有的位置
- collapse：用于表格子元素时与```display:none```效果一致；用于其他元素时与```visibility:hidden```相同
- inherit

#### 特点

- 父元素为`visibility:hidden`，子元素可为`visibility:visible`且有效。
- 无法获取焦点。
- 可在冒泡阶段响应事件——将鼠标移至`.visible`时，`.hidden`会响应`hover`事件显示

```
div{
  border: solid 2px blue;
}
.visible{
  visibility: visible;
}
.hidden{
  visibility: hidden;
}
.hidden:hover{
  visibility: visible;
}
<div class="hidden">
  I'm Parent.
  <div class="visible">
    I'm Son.
  </div>
</div>
```

- 不影响form表单提交数据
- CSS中的counter不会忽略
- Transition对`visibility`的变化有效
- visibility变化不会触发reflow——从visible设置为hidden时，不会改变元素布局相关的属性，因此不会触发reflow

### 参考资料

【1】[CSS魔法堂：display:none与visibility:hidden的恩怨情仇](https://juejin.cn/post/6844903686313869320)

## 伪类和伪元素

### 伪类(pseudo-classes)

​		其核心就是用来选择DOM树之外的信息，不能够被普通选择器选择的文档之外的元素，用来添加一些选择器的特殊效果。比如:hover、:active、:visited、:link、:visited、:first-child、:focus和:lang等

​		由于状态的变化是非静态的，所以元素达到⼀个特定状态时，它可能得到一个伪类的样式；当状态改变时，它又会失去这个样式。

​		由此可以看出，它的功能和class有些类似，但它是基于文档之外的抽象，所以叫伪类。

### 伪元素(Pseudo-elements)

​		DOM树没有定义的虚拟元素。核心就是需要创建通常不存在于文档中的元素，比如::before和::after，它选择的是元素指定内容，表示选择元素内容的之前内容或之后内容。

​		伪元素控制的内容和元素是没有差别的，但是它本身只是基于元素的抽象，并不存在于文档中，所以称为伪元素。用于将特殊的效果添加到某些选择器上。

### 伪类与伪元素的区别

- 表示方法
  - CSS2 中伪类、伪元素都是以单冒号:表示。
  - CSS2.1 后规定伪类用单冒号表示,伪元素用双冒号::表示。
  - 浏览器同样接受 CSS2 时代已经存在的伪元素(:before, :after, :first-line, :first-letter 等)的单冒号写法。
  - CSS2 之后所有新增的伪元素(如::selection)，应该采用双冒号的写法。
  - CSS3中，伪类与伪元素在语法上也有所区别，伪元素修改为以::开头。浏览器对以:开头的伪元素也继续支持，但建议规范书写为::开头。
- 定义不同
  - 伪类即假的类，可以添加类来达到效果
  - 伪元素即假元素，需要通过添加元素才能达到效果
- 总结:
  - 伪类和伪元素都是用来表示文档树以外的"元素"。
  - 在最新的规范中，伪类和伪元素分别用单冒号:和双冒号::来表示。
  - 伪类和伪元素的区别，关键点在于如果没有伪元素(或伪类)，是否需要添加元素才能达到效果，如果是则是伪元素，反之则是伪类。

### 相同之处

​		伪类和伪元素都不出现在源文件和DOM树中——也就是说在html源文件中是看不到伪类和伪元素的。

### 不同之处

- 伪类其实就是基于普通DOM元素而产生的不同状态，是DOM元素的某一特征。
- 伪元素能够创建在DOM树中不存在的抽象对象，而且这些抽象对象是能够访问到的。

### 参考资料

【1】[css 伪类与伪元素区别](https://github.com/lgwebdream/FE-Interview/issues/18#)

