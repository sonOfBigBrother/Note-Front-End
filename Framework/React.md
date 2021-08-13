[toc]

## 虚拟DOM

### 定义

Virtual DOM是对DOM的抽象，本质上是JavaScript对象，这个对象就是更加轻量级的对DOM的描述，提高重绘性能。

### 虚拟DOM的优劣

#### 优点

- 保证性能下限: 虚拟DOM可以经过diff找出最小差异，然后批量进行patch，这种操作虽然比不上手动优化，但是比起粗暴的DOM操作性能要好很多——频繁变动DOM会造成浏览器的回流或者重绘，带来一定的性能问题，因此虚拟DOM可以保证性能下限。
- 无需手动操作DOM: 虚拟DOM的diff和patch都是在一次更新中自动进行的，我们无需手动操作DOM，极大提高开发效率。
- 跨平台: 虚拟DOM本质上是JavaScript对象，而DOM与平台强相关，相比之下虚拟DOM可以进行更方便地跨平台操作，例如服务器渲染、移动端开发等等。
- 打开了函数式UI编程的大门。

#### 缺点

- 首次渲染大量DOM时，由于多了一层虚拟DOM的计算，会比 innerHTML 插入慢。虚拟DOM需要在内存中的维护一份DOM的副本。
- 单一的、频繁的更新场景下，虚拟DOM将会花费更多的时间处理计算的工作。比如，你有一个DOM节点相对较少页面，用虚拟DOM，它实际上有可能会更慢。但对于大多数单页面应用，应该都会更快。这也是React和Vue中的更新用了异步的方法，频繁更新时，只更新最后一次的。

### 虚拟DOM实现

以snabbdom.js这个库为例探究实现原理，实现流程大致为3个步骤：

- compile——把真实DOM编译成Vnode虚拟节点。（通过h函数）
- diff——通过算法，知道oldVnode和newVnode之间有什么变化。（内部diff算法）
- patch—— 把这些变化用打补丁的方式更新到真实DOM上去。

下图仅截出核心的实现代码，具体项目创建步骤可查看参考链接博客。

![](./pic/vdom/snabbdom-example.png)

此处代码的核心函数为h()和patch()，其中h函数接收3个参数，分别为DOM元素的***标签名/选择器***、***属性******和以数组形式表示的子节点信息***，最终返回一个虚拟DOM对象，对象的属性信息可查看下图

![结果截图](./pic/vdom/output.png)

patch函数接受2个参数，第一个参数表示当前视图的DOM元素或vnode，第二个参数表示需要更新的视图对应的vnode，上图代码截图中第一次调用patch时，新的vnode覆盖了原来的DOM节点（此处是id为container的div元素）——**与React中的render函数在DOM上增加节点不同**。

![page](./pic/vdom/page.png)

![element](./pic/vdom/element.png)

添加一个按钮，点击时变更ul元素的子节点信息。

![btn-change](/pic/vdom/btn-change.png)

此处patch函数将会比对旧的vnode和newVnode的信息，执行效果如下图所示。

![page-after-change](./pic/vdom/page-after-change.png)

### 参考资料

【1】[虚拟dom及dIff算法](https://juejin.cn/post/6844904078196097031)

【2】[snabbdom库](https://github.com/snabbdom/snabbdom)

## Element和Component的区别

- 元素（Element）是一个不可变、描述你想渲染的 DOM 节点或其他组件的纯对象
- 组件（Component）可以是类形式的，也可以是函数形式的，接受props作为输入，并且返回一个元素树作为输出。

## 在React中如何写注释
React 的注释和 Javascript 的多行注释很相似，它是用大括号包裹起来的。在 JSX 中这么使用：

```
render() => (<div>
  {/* 单行注释 */}
  嗨 {用户}, 写些酷酷的注释吧
  {/* 多行注释 */}
</div>)
```
## 在React中的状态提升是什么意思？

当几个组件需要共享数据时，更推荐把共享状态提升到它们最近的公共祖先。例如，如果两个组件共享相同数据时，更推荐将共享状态移到它们的父组件，而不是在各自子组件内部维护。

## setState将回调函数作为参数目的是什么？
```setState(updater[, callback])```  
根据官方文档说明，该回调函数是可选的，在setState操作完成和组件渲染完成时，会调用这个回调函数，官方建议使用componentDidUpdate()。

## React为什么用className 属性代替class?
class是JavaScript的保留关键字，而JSX是JavaScript的扩展，为了避免冲突故使用className

## React和HTML对事件处理的区别？

- 在HTML中，事件名应该是小写，然而在React中，遵循驼峰写法约定
```<!-- HTML -->
<button onclick="handleClick()">Click me</button>
/* React */
<button onClick="handleClick()">Click me</button>```
- 在HTML，返回false可以阻止默认行为发生，然而在React中，必须显示调用preventDefault 才能阻止默认行为
```<!-- HTML -->
<a href="#" onclick="console.log('The link was clicked.'); return false">Click me</a>
/* React */
function handleClick(e) {
  e.preventDefault()
  console.log("The link was clicked.")
}
```
## 在list中使用key

- key给予集合中的元素一个稳定的标识，帮助React区分元素的改变
- 如果你的元素顺序是可变的，最好避免使用索引作为key
- 如果你把列表提取为一个组件时，你应该把key提升到组件上，而不是&lt;li&gt;元素。如官网代码所示： 

## React生命周期
包含如下四个过程：

- 装载（mount）：组件第一次在DOM树中渲染的过程
- 更新（update）：组件重新渲染
- 卸载（unmount）：组件从DOM中删除的过程

### 装载过程
装载过程中涉及到的几个函数：

- constructor()
- <del>getInitialState()</del>
- <del>getDefaultProps()</del>
- <del>componentWillMount()<del>
- render()
- componentDidMount()
- static getDerivedStateFromProps(props, state)

constructor()的作用是——初始化state参数；为成员函数绑定当前组件实例。 

getInitialState()和getDefaultProps()只有在React.createClass方法创造的组件中有效，而**官方更多推崇ES6语法，因此也被逐渐废弃。**  

componentWillMount()在render()之前调用，在此方法中调用setState()不会触发渲染。官方建议在constructor()中初始化state。此外，这也是唯一一个在服务端渲染的生命周期方法。由于该方法会引起一些不安全的编码实践，**官方打算在17.0版本中移除该方法**，目前（V16.8.4）中使用UNSAFE_componentWillMount()代之。  

render()方法是组件中<strong>必须</strong>要实现的，根据this.state和this.props返回React元素、数组和fragment、portal、字符串和数值以及布尔值和null。render()是一个纯函数，不直接和浏览器交互，而是通过componentDidMount或其他生命周期方法。另，当shouldComponentUpdate()返回false时，render()不执行。  

componentDidMount()在组件装载到DOM树后执行且<strong>只执行一次</strong>，诸如从API获取数据的这类异步操作可在此方法中完成。  

getDerivedStateFromProps()在装载及后续的更新阶段调用render()前执行，返回一个修改state的对象或null。该方法只用于当组件根据props的变更修改自己内部的state的情况。
### 更新过程
更新过程涉及如下几个方法：  

- <del>componentWillReceiveProps()</del>  
- shouldComponentUpdate(nextProps, nextState)
- <del>componentWillUpdate(nextProps, nextState)</del>
- render()
- componentDidUpdate(prevProps, prevState, snapshot)  
- static getDerivedStateFromProps(props, state)
- getSnapshotBeforeUpdate(prevProps, prevState)

componentWillReceiveProps()在装载的组件接收新的props时调用。需注意的是当父组件触发子组件重新渲染时，如父组件的render()被调用，即使props没有发生变更，也要调用该方法。另，通过this.setState方法触发的更新过程不会调用该方法，因为该方法根据新的props值来确定是否需要更新state，而更新state的方法便是this.setState,如此便会引发死循环。同componentWillMount一样存在实践问题，目前使用<strong>UNSAFE_componentWillReceiveProps()代替。</strong> 

shouldComponentUpdate()在组件接收到新的props或state时、尚未渲染前调用，默认返回True。初次渲染或调用forceUpdate()时不调用。该方法通过比对this.props和nextProps、this.state和nextState来决定是否重新渲染，当返回false值时跳过更新,需注意当返回false值时不会阻止子组件状态变更引起的重新渲染，当返回true时依次调用下述三个方法。此方法意在性能优化，官方建议<b>不要在该方法下进行deepEqual或JSON.stringify()操作</b>。  

componentWillUpdate()可用于更新发生前的准备工作，初次渲染时不会被调用。不能在此方法执行this.setState()，也不能执行在方法返回前会触发更新的操作。同上理由，现已被<b>UNSAFE_componentWillUpdate()代替</b>  

render()方法同装载过程一样。  

componentDidUpdate()在更新发生后立即执行，且初次渲染时不执行。可以通过对比当前props和先前props来执行网络请求操作，如下代码所示：

若组件实现了getSnapshotBeforeUpdate()，则该方法将会返回值作为componentDidUpdate()的第三个参数snapshot。  

getSnapshotBeforeUpdate()在渲染的结果提交到DOM树之前调用，从DOM上获取更新前的一些状态信心，如滚动位置，返回值将作为componentDidUpdate的参数

### 卸载过程
卸载过程只涉及一个方法：

- componentWillUnmount()  

componentWillUnmount()在组件卸载和销毁前调用，主要执行一些清理工作，如撤销定时器和取消网络请求。

### 错误处理阶段

- componentDidCatch()

componentDidCatch: 在错误边界（实现这个方法的组件）里使用。它允许组件捕获子组件树（这个组件之下）任何地方的 JavaScript 错误，记录错误，并且用UI的信息的形式展示出来.

## Refs
Refs提供一种在render方法中访问DOM节点或React元素的方法。Refs应该少用，但是有一些好的使用例子，例如：

- 管理获取焦点，文本选择，或者媒体回放
- 触发不可避免的动画
- 集成三方DOM库  

用React.createRef()创建一个引用，然后通过ref属性附到React元素上。为了贯穿整个组件使用 refs，需要在构造器中注册ref的实例：

```js
class MyComponent extends React.Component {
  constructor(props) {
    super(props)
    this.myRef = React.createRef()
  }
  render() {
    return <div ref={this.myRef} />
  }
}
```
## React中的portal
Portal是一种子组件存在于父组件的DOM结构层级外的渲染推荐方式
```ReactDOM.createPortal(child, container)```
第一个参数（child）是可渲染的React子元素，例如，一个元素，字符串或文档片段。第二个参数（container）是一个DOM元素	

