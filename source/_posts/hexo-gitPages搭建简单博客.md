---
title: hexo+gitPages搭建简单博客
date: 2018-05-26 09:02:31
tags: 
  - hexo
---

### 基于hexo和gitPages搭建的博客

只是用来记录一些工作心得，所以怎么简单怎么来
<!-- more -->
### hexo 常用命令

1. hexo generate (hexo g)  生成一个存放静态文件的文件夹
2. hexo server (hexo s)  启动本地服务器预览博客内容
3. hexo deploy  当前目录文件下生成.deploy_git文件夹,并且把生成的静态文件上传到github上,需提前写好_config.yml
4. hexo new post 文章名 在根目录下的source/_posts下生成一个md文件,编辑此文件写文章
5. hexo clean  清除缓存文件 db.json 和 生成的静态文件夹 public

---
### 常用组合命令
hexo clean + hexo g + hexo s 本地预览博客文章
hexo clean + hexo g + hexo d 发布到github上
*hexo g/hexo d合并时候简写命令为 hexo g -d

---
### hexo遇到的问题
1.全局安装后提示hexo不是命令提示符，解决办法:
①用git Bash 安装即可
②将安装包的位置添加到环境变量中

2.hexo d 提示 error deployer not found:github, 解决办法:
安装hexo-deployer-git // npm install hexo-deployer-git --save

## hexo使用了next主题

1.新增tags标签页
① hexo new page "tags"  新增一个tags页面,此时在source下生成一个tags文件夹,里面包含一个index.md
② 编辑刚才生成的这个index.md页面 新增 type: 'tags'
③ 进入主题配置文件(_config.yml) 新增tags
```
menu:
  home: /|| home
  tags: /tags/|| tags
```
---
2.新增搜索功能
① hexo根目录下安装 npm install hexo-generator-searchdb --save
② 修改配置文件(theme/next/_config.yml)

```
local_search:
  enable: true #默认为false
```
③ 配置文件中新增

```
search:
  path: search.xml
  field: post #搜索范围 post文章
  format: html
  limit: 1000 #搜索限制条数
```