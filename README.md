##npm私有仓库搭建


####目的：
Node.js开发本地项目，有时不同项目之间存在依赖，如果不想把项目发布到npm社区的仓库，则需要有自己本地的仓库。

####构建工具：sinopia（主流）

安装命令：npm install -g sinopia 或者 yarn global add sinopia
安装遇到问题：node-gyp依赖python 2.7。
安装python2.7，并把它添加到环境变量PATH,npm config set python python2.7。

####启动： sinopia
```javascript
 warn  --- config file  - /home/<user>/.config/sinopia/config.yaml
 warn  --- http address - http://localhost:4873/
```
 * 从上面命令输出可以看到配置文件路径：/home//.config/sinopia/config.yaml

 * 下面修改配置文件，在最后加上一行 “listen: 0.0.0.0:4873”，目的是为了可以从别的机器远程上也能访问 sinopia 仓库。
 修改完成后再次启动 Sinopia 服务。

* 如何持续保持这个进程？需要守护进程。

PM2是node进程管理工具，可以利用它来简化很多node应用管理的繁琐任务，如性能监控、自动重启、负载均衡等，而且使用非常简单。

```javascript
安装：npm install -g pm2
启动程序：pm2 start <app_name|id|all>
　　可以指定应用名称 pm2 start app,js --name=test

列举进程：pm2 list
退出程序：pm2 stop <app_name|id|all>
重起应用：pm2 restart
程序信息：pm2 describe id|all
监控：pm2 monit
实时集中log处理: pm2 logs
```


####私有仓库的使用
添加私有仓库
$ nrm add mynpm http://192.168.0.123:4873

使用私有仓库
$ nrm use mynpm

测试私有仓库
```javascript
$ mkdir test
$ cd test
$ npm install webpack # 第一次安装比较慢
...

$ rm -rf webpack
$ npm install webpack # 第二次安装就比较快了
...
```

###nrm
开发的npm registry 管理工具 nrm,  能够查看和切换当前使用的registry

安装 npm install -g nrm
```javascript
nrm ls
  npm ---- https://registry.npmjs.org/
  cnpm --- http://r.cnpmjs.org/
  taobao - https://registry.npm.taobao.org/
  nj ----- https://registry.nodejitsu.com/
  rednpm - http://registry.mirror.cqupt.edu.cn/
  npmMirror  https://skimdb.npmjs.com/registry/
  edunpm - http://registry.enpmjs.org/
```
使用： nrm use cnpm
```javascript
nrm help // show help
nrm list // show all registries
nrm use cnpm // switch to cnpm
nrm home // go to a registry home page
```

####使用npm参考：
注册一个新用户
```javascript
npm adduser --registry=http://npm.oneapm.me:9090
```
命令执行后，按照要求输入用户名、密码和邮箱，即可注册成功。注意密码是不显示的，输入完成后回车即可。

配置了全局的 NPM 库之后，可以省略 --registry 参数；在添加了 .npmrc 配置文件后，在其目录及其子目录下，也可以省略这一参数。以下命令相同，不再进行说明。
登录到服务器
```javascript
npm login --registry=http://npm.oneapm.me:9090
```
按照提示信息输入完成后，当前控制台窗口即是成功登录的状态。需要注意的是，登录成功的状态只在当前控制台窗口有效。如果需要在另一个窗口中操作，还需要执行一遍登录命令。
发布／更新包

在项目目录下，登录成功后，使用如下命令就可以发布包：
```javascript
npm publish --registry=http://npm.oneapm.me:9090
```
更多 NPM 命令行的参数，可以查看 NPM 官方文档。
