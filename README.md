# OnlineJudge Front End
[![vue](https://img.shields.io/badge/vue-2.5.13-blue.svg?style=flat-square)](https://github.com/vuejs/vue)
[![vuex](https://img.shields.io/badge/vuex-3.0.1-blue.svg?style=flat-square)](https://vuex.vuejs.org/)
[![echarts](https://img.shields.io/badge/echarts-3.8.3-blue.svg?style=flat-square)](https://github.com/ecomfe/echarts)
[![iview](https://img.shields.io/badge/iview-2.8.0-blue.svg?style=flat-square)](https://github.com/iview/iview)
[![element-ui](https://img.shields.io/badge/element-2.0.9-blue.svg?style=flat-square)](https://github.com/ElemeFE/element)
[![Build Status](https://travis-ci.org/QingdaoU/OnlineJudgeFE.svg?branch=master)](https://travis-ci.org/QingdaoU/OnlineJudgeFE)

>### A multiple pages app built for OnlineJudge. [Demo](https://qduoj.com)

## Features

+ Webpack3 multiple pages with bundle size optimization
+ Easy use simditor & Nice codemirror editor
+ Amazing charting and visualization(echarts)
+ User-friendly operation
+ Quite beautiful：)

## Get Started

Install nodejs **v8.12.0** first.

```bash
npm install
# we use webpack DllReference to decrease the build time,
# this command only needs execute once unless you upgrade the package in build/webpack.dll.conf.js
NODE_ENV=development npm run build:dll

# the dev-server will set proxy table to your backend
export TARGET=http://Your-backend

# serve with hot reload at localhost:8080
npm run dev
```

## Screenshots

[Check here.](https://github.com/QingdaoU/OnlineJudge)

## Browser Support

Modern browsers and Internet Explorer 10+.

## LICENSE

[MIT](http://opensource.org/licenses/MIT)



# OJ服务器搭建流程

>### 开发环境用于前端更改（本地或服务器），生产环境用于搭建后端（服务器）

## 开发环境开始（测试系统为Ubuntu14-64bit)

1.安装环境初始化
```bash
sudo apt-get update
sudo apt-get install git vim wget
sudo apt-get install redis-server
sudo apt-get install postgresql
sudo apt-get install openssl libssl-dev
sudo apt-get install build-essential 
```

2.获取前端项目源代码
```bash
git clone https://github.com/JehuYu/OJ-fontend.git #github上的前端链接
```

3.安装nvm和nodejs
```bash
wget -O- https://raw.githubusercontent.com/creationix/nvm/v0.33.0/install.sh | bash
```
>完成后重启服务器，此时nvm才会生效
```bash
nvm install 6.11.0
nvm use 6.11.0
node --version
```

4.部署前端
>cd到之前clone到的文件根目录
```bash
npm install
#若有错误，删除干净nvm nodejs npm node_modul，重新开始
NODE_ENV=development npm run build:dll
export TARGET=http://Your-backend
npm run dev
```

>至此，可登陆8080端口查看前端效果，若确认无误，使用Ctrl+C退出。

## 生产环境搭建（测试环境为Docker-18-Ubuntu-18)

1.开始安装
```bash
git clone -b 2.0 https://github.com/QingdaoU/OnlineJudgeDeploy.git && cd OnlineJudgeDeploy
```
2.docker服务启动
```bash
docker-compose up -d
```
>可运行docker ps -a来查看，若为healthy，则成功。


## 前端更改

### 开发环境处理
#### clone的根目录下输入命令提取dist
```bash
NODE_ENV=production npm run build:dll
npm run build
```
>获取dist文件夹，放置于生产环境服务器文件根目录

### 生产环境处理
### 运行代码，以下代码中的oj-backend都为该名所对应的docker容器的ID，通过docker ps -a查看。
```bash
docker cp ./dist f1f6e014869b:/app/
#进入终端重启服务
docker exec -it f1f6e014869b /bin/sh
#在# /app的提示符下运行
cd deploy/
./entrypoint.sh
#服务器重启后
exit
```
