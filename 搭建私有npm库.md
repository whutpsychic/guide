# 搭建私有npm库
​	

## 概述

本文描述了在一台windows服务器上搭建私有npm库的步骤。

搭建私有npm库的目的主要有2个，其一是利用内网来提高开发人员使用`npm install`命令下载包的速度，其二是允许上传自己编写的包，使团队的组件管理更加规范。

本文主要描述搭建npm库的流程，而使用私有npm库的方式请参考另一篇文档：[使用私有npm库](使用私有npm库.md)

​	

##  安装node.js

远程登录windows服务器，安装node.js。

安装完成后，执行以下命令修改仓库地址。

```shell
# 修改npm的registry地址
npm config set registry https://registry.npm.taobao.org
```

​	

## 安装sinopia

执行以下命令安装sinopia

```shell
# 全局安装sinopia
npm i sinopia -g
```



## 运行sinopia

执行以下命令来启动sinopia

```shell
# 启动sinopia
sinopia
```

​	

打开浏览器，访问http://localhost:4873，如果能访问，则表示sinopia运行成功。

​	

## 修改配置

sinopia启动后，可以看到命令行中提示了配置文件config.yaml的路径，根据路径打开该文件。

​	

其中的`storage`指定了库文件保存地址，可以修改为其它文件夹，例如：

```yaml
storage: E:\sinopia
```

​	

修改`url`，指向淘宝镜像：

```yaml
uplinks:
  npmjs:
    url: https://registry.npm.taobao.org
```

​	

添加`listener`，在配置文件最底部添加以下配置：

```yaml
# you can specify listen address (or simply a port) 
listen: 0.0.0.0:4873  #默认没有，只能在本机访问，添加后可以通过外网访问
```



## 访问sinopia

在浏览器中打开`http://xxx:4873`，确保网站可以访问，这里`xxx`是安装sinopia的服务器的地址。



将registry指向sinopia服务，指令如下：

```shell
npm config set registry http://xxx.4873
```



随意下载一个npm包，试试能不能下载成功。

```shell
# 随意新建一个文件夹，进入其中并执行以下命令
npm init
npm i react -s
```

如果可以下载成功，则表示sinopia配置成功。



## 附录：一个真实的config.yaml配置实例

```yaml
#
# This is the default config file. It allows all users to do anything,
# so don't use it on production systems.
#
# Look here for more config file examples:
# https://github.com/rlidwka/sinopia/tree/master/conf
#

# path to a directory with all packages
storage: E:\sinopia

auth:
  htpasswd:
    file: ./htpasswd
    # Maximum amount of users allowed to register, defaults to "+inf".
    # You can set this to -1 to disable registration.
    #max_users: 1000

# a list of other known repositories we can talk to
uplinks:
  npmjs:
    url: https://registry.npm.taobao.org

packages:
  '@*/*':
    # scoped packages
    access: $all
    publish: $authenticated

  '*':
    # allow all users (including non-authenticated users) to read and
    # publish all packages
    #
    # you can specify usernames/groupnames (depending on your auth plugin)
    # and three keywords: "$all", "$anonymous", "$authenticated"
    access: $all

    # allow all known users to publish packages
    # (anyone can register by default, remember?)
    publish: $authenticated

    # if package is not available locally, proxy requests to 'npmjs' registry
    proxy: npmjs

# log settings
logs:
  - {type: stdout, format: pretty, level: http}
  #- {type: file, path: sinopia.log, level: info}

  
# you can specify listen address (or simply a port) 
listen: 0.0.0.0:4873  #默认没有，只能在本机访问，添加后可以通过外网访问

# you can specify proxy used with all requests in wget-like manner here
# (or set up ENV variables with the same name)
http_proxy: http://192.168.62.2:3128/  # 设置代理服务器
https_proxy: http://192.168.62.2:3128/
```



## 常见问题

### 如果服务器使用了代理怎么配置？

如果服务器使用代理，在配置npm时需添加proxy的设置

```shell
# 修改npm的proxy地址
npm config set proxy ......
npm config set https-proxy ......
```

如果不进行以上配置，会发生`connect ETIMEOUT`错误。

​	

在sinapia的`config.yaml`文件中，也需要添加相关配置：

```
# you can specify proxy used with all requests in wget-like manner here
# (or set up ENV variables with the same name)
http_proxy: ...... # 设置代理服务器
https_proxy: ......
```





