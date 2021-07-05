## Tree-Shaking实现

在解析流程中的tree-shaking实现之前，我们首先要了解两点前置知识：

- rollup中的tree-shaking使用[acorn](https://github.com/acornjs/acorn)实现AST抽象语法树的遍历解析，acorn和babel功能相同，但 acorn更加轻量，在此之前AST工作流也是必须要了解的；
- rollup使用[magic-string](https://github.com/rich-harris/magic-string)工具操作字符串和生成source-map。

### 实现原理

- rollup()阶段，解析源码，生成AST tree，对AST tree上的每个节点进行遍历，判断出是否 include(标记避免重复打包)，是的话标记，然后生成chunks，最后导出。
- generate()/write()阶段，根据rollup()阶段做的标记，进行代码收集，最后生成真正用到的代码。

### 参考资料

[无用代码去哪了？](https://mp.weixin.qq.com/s?__biz=MzU4MTc2NTc5NQ==&mid=2247487136&idx=1&sn=83ed12ba9b9e661a3727ab977916a2ab&chksm=fd43d16cca34587a3a0314d78f32f2536dc621a616f28810560a294b87b72753830caffbd974&scene=21#wechat_redirect)

