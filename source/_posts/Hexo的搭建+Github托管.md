---
title: Hexo的搭建以及交给Github托管
date: 2019-07-01 00:49:54
tags: [hexo]
description:  第一搭建hexo博客，做一次记录
---
## 搭建Hexo  
1. 在 nodejs.org 下载Node.js长期支持版。闭眼安装
2. 检查nodejs是否成功安装  
node -v;　　npm -v
3. 更换安装管理器源为cnpm，提高下载速度  
`npm install -g cnpm --registry=https://registry.npm.taobao.org`
<!--more-->
4. 安装hexo  
`cnpm install -g hexo-cli`
5. 新建一个目录存放hexo  
`md blog`
6. 初始化一个博客  
进入新建的目录然后 `hexo init`
7. 启动hexo服务  
`hexo s`
根据提示地址进入，就可预览

## 交由Github托管
1. 创建一个可访问的空仓库
bigzom.github.io
用户名.github.io 必须这样才能访问
2. 安装git管理插件 
cnpm install --save hexo-deployer-git
如果未安装 执行hexo d 命令时会报 ERROR Deployer not found: git 的错误
3. 修改blog目录下的_config.yml 文件
到最底部，<font color=red>注意要有空格</font>
````yml
deploy:
  type: git
  repository: https://github.com/bigzom/bigzom.github.io.git
  branch: master
````
4. 部署到远端  
hexo d  (deploy)  
访问 bigzom.github.io 即可访问

## 进阶
#### 更换主题
1. 推荐主题  
https://github.com/litten/hexo-theme-yilia
2. 克隆主题  
git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
3. 再次部署  
hexo d

#### 基本命令
1. cmd
dir 查看目录
md 创建别
2. hexo  
hexo init 初始化博客  
hexo clean 清除缓存文件 db.json 和已生成的静态文件 public 
hexo g 生成缓存文件 db.json 和静态文件 public
hexo s 启动本地服务  方便在本地看
hexo d 自动生成静态文件并部署到远程仓库 
3. hexo 手动截断
`<!-- more --> `
4. hexo 存放博客的位置  
source\_posts
5. yilia主题设置头像  
 将头像放在source文件夹即可，在yiliya的配置文件中修改头像地址 avatar: /图片名.后缀  


