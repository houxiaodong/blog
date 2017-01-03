---
title: hexo 常用命令
date: 2016-11-01 16:32:38
categories: 工具
tags:  
    - hexo
---
![](/images/start7.jpg)  
由于本人很久时间没有写博客，所以以前学习的很多关于hexo的命令知识点都忘了，所以下面罗列一下。
# Hexo 安装、升级以及初始化 #
---
	
	npm install hexo -g  #安装
	npm update hexo -g  #升级
	hexo init  #初始化
# hexo 各种命令简写 #
---
	hexo n "博客名称"  => hexo new "博客名称"   #这两个都是创建新文章，前者是简写模式
	hexo p  => hexo publish 
	hexo g  => hexo generate  #生成
	hexo s  => hexo server  #启动服务预览
	hexo d  => hexo deploy  #部署
# Hexo服务器 #
---
	hexo server   #Hexo 会监视文件变动并自动更新，无须重启服务器。
	hexo server -s   #静态模式
	hexo server -p 5000   #更改端口
	hexo server -i 192.168.1.1   #自定义IP
	hexo clean   #清除缓存，网页正常情况下可以忽略此条命令
	hexo g   #生成静态网页
	hexo d   #开始部署
# Hexo 监视文件变动 #
---
	hexo generate   #使用Hexo生成静态文件
	hexo generate --watch   #监视文件变动
# Hexo 完成后部署 #
---
	两个命令的作用是相同的
	hexo generate --deploy
	hexo deploy --generate
	hexo deploy -g
	hexo server -g
# Hexo 草稿 #
---
	hexo publish [layout] <title>
# Hexo 模板 #
---
	title: 博客模板
	date: 2016-11-01 16:32:43
	categories: 工具
	tags:
		- 博客模板
# Hexo Next主题设置文章摘要 #
---
	# Automatically Excerpt. Not recommand.
	# Please use <!-- more --> in the post to control excerpt accurately.
	auto_excerpt:
	  enable: true
	  length: 150
	或者是用以下方法：
	以上是文章摘要 <!--more--> 以下是余下全文 
# Hexo 写文章命令流程 #
---
	
	hexo new "postName" #新建文章
	hexo new page "pageName" #新建页面
	hexo generate #生成静态页面至public目录
	hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
	hexo deploy #将.deploy目录部署到GitHub
# Hexo 写作命令 #
---
	hexo new page <title>
	hexo new post <title>
# hexo 推送到服务器上 #
---
	hexo n #写文章
	hexo g #生成
	hexo d #部署 #可与hexo g合并为 hexo d -g