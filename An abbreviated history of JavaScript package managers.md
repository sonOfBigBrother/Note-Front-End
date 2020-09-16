### Glossary(术语)

- Package: A folder that contains any number of files and folders. It's not just a folder because it must include one file with a standard name. For example, npm requires this file to be named package.json 
- Module:  A file or group of files that can be imported or exported into another file.
- Library: Technically,the same thing as a module
- Dependency : A dependency refers to anything required for your code to work
- Sub-dependency: A dependency of a dependency
- Bare Specifier: A file or folder name without a provided path
- Package Registry: a database where packages are stored.
- Package Manager: A tool that's supposed to make it easier to install, update, uninstall and configure the packages you use in your program

### npm

#### 版本演化

npm v2版本中每个依赖各自安装子依赖到node_modules目录下，v3开始默认所有依赖安装到项目的node_modules下，不同版本的重复子依赖则下载到对应的依赖node_modules目录下。

![img](https://miro.medium.com/max/1570/1*cEgwty6roKSgdZioUMVa3g.png)

<b><i>npm install --save loadsh</i></b> 

```json
"dependencies": {
    "lodash": "^4.17.4"
}
```

默认以脱字符^开头，表明每次安装以major版本为4的依赖库，这将导致每个人安装的版本可能会有所差异。

### Yarn

yarn的速度明显快于npm，这也是其迅速赢得广大开发者青睐的原因。

![img](https://miro.medium.com/max/1496/1*29Ukg0NNLM-AY31STr5UKA.png)