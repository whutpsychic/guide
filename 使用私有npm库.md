# 使用私有npm库
​	

## 概述

本文介绍私有npm库的使用方法。

使用私有npm库，可以提高`npm install`的下载速度，而且允许上传自己编写的npm包。

我们使用`sinopia`创建npm库，创建成功后，会有一个访问地址`http://xxx:4873`。关于`sinopia`的搭建步骤，可以参考另一篇[搭建私有npm库](搭建私有npm库.md)

​	

## 访问私有npm库

在浏览器中打开sinopia的地址：`http://xxx:4873`，确保网站可以访问。



将registry指向该地址，指令如下：

```shell
npm config set registry http://xxx.4873
```



随意下载一个npm包，试试能不能下载成功。

```shell
# 随意新建一个文件夹，进入其中并执行以下命令
npm init
npm i react -s
```

如果可以下载成功，则表示访问成功。

​	

## 下载npm包

从私有库下载与从官方下载是没什么区别的，也是使用`npm install`或`npm i`命令。值得注意的是，第一次下载某个包时，需要去官网获取，因此比较慢；第二下载时，可以直接从本地的sinopia服务器获取，下载速度会快很多。



## 发布npm包

可以把自己编写的npm包上传到私有npm库中，步骤如下：

创建用户

```shell
npm adduser
```

会提示输入账号、密码和邮箱。

​	

新建一个文件夹，在其中创建文件`index.js`，该js文件只包含一行代码：

```js
module.exports = { message: 'Hello World!' };
```

这是一个最简单的js模块，输入一个对象，包含属性`x`。

​	

在文件夹内执行以下命令：

```shell
npm init
npm publish
```

​	

查看sinopia的网址：`http://xxx:4873`，可以看到我们上传的包出现在其中。



下面实际测试一下我们发布的包。

使用create-react-app构建测试项目，具体指令如下：

```shell
# 使用create-react-app进行测试
create-react-app test

# 进入创建出来的test文件夹，然后执行
npm start

# 安装之前发布的包，这里xxx指的是发布包的名字
npm i xxx -s
```

打开src/index.js文件，将里面的内容修改如下：

```javascript
import React, { Component } from 'react';
import testpkg from 'testpkg';

class App extends Component {
  render() {
    return (
      <div className="App">
        {testpkg.message}
      </div>
    );
  }
}

export default App;
```

查看http://localhost:3000，发现页面中成功显示了之前发布组件中定义的字符串`Hello World!`



## 更新npm包

打开之前发布包的`package.json`文件，将其中version字段变更为`1.0.1`。

打开`index.js`文件，将其中内容修改如下：

```javascript
module.exports = { message: 'nihao' };
```

​	

执行以下命令：

```shell
npm publish
```

 

查看sinopia的网址：`http://xxx:4873`，可以看到包的版本号变为`1.0.1`。



下面进行测试，进入之前创建的create-react-app测试目录，执行如下命令：

```
# 更新包
npm update xxx

# 重新启动项目（更新包后，需要重新启动才会更新）
npm start
```

查看http://localhost:3000，发现页面中显示的内容发生了变化



## 删除npm包

使用如下命令删除npm包

```
# 删除包
npm unpublish xxx --force
```

