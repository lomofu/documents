# React 进阶

通过简单引入js文件，过了一遍React基本知识点和jsx。接下来开始使用脚手架，通过实际中工程上手。



## 1.脚手架

### Create React App

> #### **Create React App** 是一个用于**学习 React** 的舒适环境，也是用 React 创建**新的单页应用**的最佳方式。
>
> #### 它会配置你的开发环境，以便使你能够使用最新的 JavaScript 特性，提供良好的开发体验，并为生产环境优化你的应用程序。

说白了就是类似vue推出的vue cli，会帮你配置一些工具链中需要的东西，不用每一次都进行繁琐的配置

执行

```bash
npm install -g create-react-app
create-react-app 项目名
```

或者

```bash
npx create-react-app 项目名
```

>#### 1.上面第一个命令是全局安装create-react-app这个脚手架，之后创建react项目 使用create-react-app 项目名
>
>#### 2.第二个是执行npm安装create-react-app后 再执行create-react-app 项目名。就是将两个命令合二为一了

如果出现yarn超时错误：

```bash
yarn config set registry https://registry.npm.taobao.org
```

