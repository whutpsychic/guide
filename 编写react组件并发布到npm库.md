# 编写react组件并发布到npm库
​	

## 概述

本文讲解编写react组件的步骤，并把编写的组件发布到npm库中。

在本文中，我们将编写一个名为`react-file-selector` 的组件，该组件实现点击或拖拽来选择上传文件的功能。

组件编写完成后，我们把它发布到私有npm库中，请确保已经按照[使用私有npm库](使用私有npm库.md)中的步骤配置好相关环境。

​	

## github创建项目

组件的源代码一律在github维护，首先在[https://github.com](https://github.com/) 注册账户，新建项目，然后clone到本地。

​	

## 初始化

在项目根目录执行以下命令：

```shell
# 初始化项目
npm init

# 按照以下的规则进行配置
# name: tj-react-file-selector
# version: 0.1.0
# description: react文件选择
# entry point: dist/index.js
# test command:
# git repository: 
# keywords: 
# author:
# license:
```

执行后，在项目根目录会新建一个文件`package.json`，该文件用于描述模块的信息。



## 编写组件



我们要编写的`react-file-selector`组件需要依赖其他组件，包括`react`，`react-dom`，以及一个第三方库`react-dropzone`，可以从`npm`下载这些组件。

​		

首先，在项目根目录执行以下命令来下载`react`库：

```shell
# 下载react
npm install react --save
npm install react-dom --save
```



然后引入第三方库`react-dropzone`，用于实现文件的拖拽：

```shell
# 下载第三方库
npm install react-dropzone --save
```

​	

在项目根目录创建`src`文件夹，并在其中新建文件`main.js`。在`main.js`中编写以下代码：

```javascript
import React, { Component } from 'react';
import Dropzone from 'react-dropzone'

class FileSelector extends Component {

  render() {
    const { file, onSelectFile } = this.props;

    return (
      <div className="react-file-selector">
        <Dropzone style={{}} onDrop={(files) => {
          let f = (files && files.length) ? files[0] : null;
          onSelectFile(f);
        }}>
          <p>
            {(function (f) {
              if (f) {
                let str = f.name + ' - ' + f.size + '字节';
                return str;
              } else {
                return '点击此处选择文件，您也可以直接拖拽文件至此';
              }
            })(file)}
          </p>
        </Dropzone>
      </div>
    )
  }
}

export default FileSelector;
```



## 配置babel

在刚才编写的`main.js`中，使用ES6和jsx语法，因此需要安装babel，将代码编译为ES5。

使用以下命令安装babel

```shell
# 安装babel
npm install babel-cli --save-dev
npm install babel-core --save-dev
npm install babel-loader --save-dev
npm install babel-preset-env --save-dev
npm install babel-preset-react --save-dev
```

​	

在根目录创建文件`.babelrc`，并在其中编写以下内容

```json
{
  "presets": [ "env", "react" ]
}
```



## 配置webpack

使用webpack作为代码打包工具。

首先安装webpack：

```shell
# 安装webpack
npm install webpack --save-dev
```

​	

在根目录创建文件`webpack.config.js`，并在其中编写以下内容：

```javascript
var path = require('path');

module.exports = {
  entry: path.join(__dirname, 'src', 'index.js'),
  output: {
    path: path.join(__dirname, 'dist'),
    filename: 'index.js'
  },
  module: {
    loaders: [{
      test: /\.js$/,
      loader: 'babel-loader',
      include: [
        path.join(__dirname, 'src')
      ]
    }]
  }
}
```



## 配置编译脚本



接下来要配置编译项目的脚本，一般我们把预定义的脚本写在`package.json`中，方便以后执行。

​	

首先安装第三方模块`rimraf`，用于删除文件夹：

```shell
# 安装rimraf
npm install rimraf --save-dev
```



打开`package.json`，找到"scripts"字段，将其修改如下：

```json
"scripts": {
    "clean": "rimraf ./dist",
    "build": "npm run clean && webpack"
},
```

​	

## 编译

执行以下命令：

```shell
# 编译项目
npm run build
```

​	

查看`dist`文件夹，其中生成了`index.js`文件。



## 配置开发环境

我们希望在组件开发过程中，实时测试组件效果。我们将编写一个`index.html`页面，在该页面引用我们编写的组件，在浏览器中查看其效果。

​	

首先安装相关的模块

```shell
# 安装webpack-dev-server
npm install webpack-dev-server --save-dev

# 安装copy-webpack-plugin
npm install copy-webpack-plugin --save-dev
```

​	

在项目根目录新建文件夹`examples`，在其中新建文件`index.html`，编写如下代码：

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
  <title>Test</title>
  <link rel="stylesheet" href="index.css" />
</head>
<body>
  <div id="root"></div>
  <script src="index.js"></script>
</body>
</html>
```

​	

在`examples`文件夹中新建文件`index.js`，其中代码如下：

```javascript
import React, { Component } from "react";
import ReactDOM from "react-dom";
import FileSelector from '../src/index.js';

class App extends Component {

  constructor() {
    super();
    this.state = { file: null };
  }

  render() {
    return (
      <div>
        <FileSelector
          file={this.state.file}
          onSelectFile={this.onSelectFile.bind(this)}
        />
      </div>
    );
  }
  onSelectFile(f) {
    this.setState({ file: f });
  }
}

ReactDOM.render(<App />, document.getElementById('root')); 
```

​	

在`examples`文件夹中新建文件`index.css`，其中代码如下：

```css
.react-file-selector {
}

.react-file-selector > div {    
    height: 200px;
    border: 2px dashed #666;
    margin-left:10px;
    margin-right:10px;
    margin-bottom:20px;
}

.react-file-selector > span {
    font-size: 20px;
    font-family: 'Microsoft YaHei';
}

.react-file-selector p {
    font-size: 20px;
    font-family: 'Microsoft YaHei';
}
```



在`examples`文件夹中新建文件`webpack.config.dev.js`，在其中编写如下代码：

```javascript
var path = require('path');
var CopyWebpackPlugin = require('copy-webpack-plugin');

module.exports = {
  entry: path.join(__dirname, 'index.js'),
  output: {
    path: path.join(__dirname, '../examples-output'),
    filename: 'index.js'
  },
  devtool: 'source-map',
  module: {
    loaders: [{
      test: /\.js$/,
      loader: 'babel-loader',
      include: [
        path.join(__dirname),
        path.join(__dirname, '../src')
      ]
    }]
  },
  devServer: {
    open: true,
    port: 9901
  },
  plugins: [
    new CopyWebpackPlugin([
      { from: path.join(__dirname, 'index.html') },
      { from: path.join(__dirname, 'index.css') }
    ])
  ]
}
```

​	

打开`package.json`，找到"scripts"字段，将其修改如下：

```json
"scripts": {
    "clean": "rimraf ./dist",
    "build": "npm run clean && webpack",
    "dev": "webpack-dev-server --config ./examples/webpack.config.dev.js",
    "start": "rimraf ./examples-output && npm run dev"
  },
```

​	

执行以下命令：

```shell
# 启动开发环境
npm start
```

执行`npm start`后，会自动弹出浏览器窗口，并指向`localhost:9901`。

​	

## 发布组件

组件编写完成后，就可以发布到npm库中了。每次发布前，需要在`package.json`中修改版本号。

​	

打开`package.json`，找到"scripts"字段，将其修改如下：

```json
"scripts": {
    "clean": "rimraf ./dist",
    "build": "npm run clean && webpack",
    "dev": "webpack-dev-server --config ./examples/webpack.config.dev.js",
    "start": "rimraf ./examples-output && npm run dev",
    "prepublish": "npm run build"
},
```

​	

执行以下命令：

```shell
# 启动开发环境
npm start
```













