## 问题汇总
### 修改npm全局安装的默认路径
以 Windows 系统为例，默认情况下，npm 全局模块安装的路径为：C:\Users\[username]\AppData\Roaming\npm 下，cache 的路径为C:\Users\[username]\AppData\Roaming\npm_cache。在选定的目录下新建两个目录：node_global 和 node_cache，打开命令行窗口，输入下面两行命令：
> npm config set prefix "选定目录\node_global"  
> npm config set cache "选定目录\node_cache"

可用如下两条命令查看设置是否生效：
>npm config get prefix  
>npm config get cache

最后在 Windows 的系统环境变量`PATH`中新增一个值，即为上文中"选定目录\node_global"

### nrm use 切换镜像源失败

在项目中执行`nrm use taobao`切换镜像源时，发现提示如下信息，尝试切换到其他镜像，提示信息依旧。

> Registry has been set to: https://registry.npmjs.org/

#### 分析及解决

查看项目，应该能发现一个 .npmrc 文件，写有如下的配置信息。

```bash
registry=https://registry.npmjs.org
```

**虽然 rnm 设置了镜像源，但是当前项目中 .npmrc 有设置时，.npmrc 中优先。可在其他目录下进行切换操作**