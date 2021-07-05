## 问题汇总
### 修改npm全局安装的默认路径
以Windows系统为例，默认情况下，npm全局模块安装的路径为：C:\Users\[username]\AppData\Roaming\npm下，cache的路径为C:\Users\[username]\AppData\Roaming\npm_cache。在选定的目录下新建两个目录：node_global和node_cache，打开cmd，输入下面两行命令：
> npm config set prefix "选定目录\node_global"  
> npm config set cache "选定目录\node_cache"

可用如下两条命令查看设置是否生效：
>npm config get prefix  
>npm config get cache

最后在Windows的系统环境变量PATH中新增一个值，即为上文中"选定目录\node_global"