# CSS核心知识点

- 选择器选择规则
- 继承
- 盒模型
- 块级格式化上下文（BFC）
- 层叠上下文
- link和@import的区别

---
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
每个box由4个部分组成：内容（content）、内边距（padding）、边框（border）和外边距（margin）  
<img src="https://mdn.mozillademos.org/files/8685/boxmodel-(3).png" alt="CSS Box model" style="height: 384px; width: 548px;">  
<b><i>box-sizing</i></b>属性可以更改默认的CSS盒子模型，默认值为<i>content-box</i>，当前元素的宽度即content的宽度，若加上padding和border的宽度大于父元素宽度则会产生溢出问题。当设为<i>border-box</i>且设置该元素宽度时，content、padding、border的宽度之和为该宽度值，另规定content的最小宽度为0。  
<b><i>Note:</i></b>  
在Firefox中实现了padding-box，即width = content_width+padding_width;height = content_height+padding_height,但在<b>Firefox 50</b>中已被删除。  
box-sizing中没有margin-box设定值，margin本身并不改变元素盒子的尺寸，没有价值且使用场景有限，见<a href="https://www.zhangxinxu.com/wordpress/2016/09/talking-about-css-margin-box/">张旭鑫的文章</a> 

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
```
.parent{
    width: -moz-fit-content;
    width: -webkit-fit-content;
    width:fit-content;
    margin:0 auto;
}
```
- 使用flex + justify-content:
```
.son{
    display: flex;
    justify-content: center;
}
```
- 使用flex 2009年的语法，与前述2012年的标准flex语法不同，是需要使用前缀的：
```
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
```
.son{
    position:absolute;
    left:50%;
    transform:translate(-50%,0);
}
```
### 垂直居中

- 若是单行文本，设置<i>line-height</i>等于父元素高度。
- 若是行内块级元素，使用<i>vertical-align</i>和伪元素实现：
```
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
```
.parent {
  display: flex;
  align-items: center;
}
```
- 父元素设置相对定位，子元素使用transform：
```
.son{
    position:absolute;
    top:50%;
    -webkit-transform: translate(-50%,-50%);  
    -ms-transform: translate(-50%,-50%);
    transform: translate(-50%,-50%);
}
```
- 父元素设置相对定位，子元素使用：
```
.son{
    position:absolute;
    height:固定值;
    top:0;
    bottom:0;
    margin:auto 0;
}
```