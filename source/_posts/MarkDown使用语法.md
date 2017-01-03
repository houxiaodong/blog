---
title: MarkDown使用语法
date: 2016-11-01 16:32:25
categories: 工具
tags: 
    - MarkDown
---
![](/images/start1.jpg)  
根据资料总结Markdown的优点有如下几点：
MarkDown是纯文本，它的兼容性极强，几乎可以使用用所有文本编辑器打开并且编辑。专业的markdown编辑器可以让你专注于文字的编辑而不用关心排版布局。它的格式转换很方便方便，它的文本可以转换为html、电子书等。它的标语法有很好的可读性。
# 标题的几种格式： #
平时常用的的文本编辑器中大多是这样模式：输入文本、选中文本、设置标题格式等。
而在 Markdown 中，你只需要在文本前面加上 # 即可，同理、你还可以增加二级标题、三级标题、四级标题、五级标题和六级标题，总共六级，只需要增加 # 即可，标题字号相应降低。例如：  
```

	# 这是H1 #  
	## 这是H2 ##    
	### 这是H3 ###    
	#### 这是H4 ####     
	##### 这是H5 #####    
	###### 这是H6 ######    
```
# 这是H1 #
## 这是H2 ##
### 这是H3 ###
#### 这是H4 ####
##### 这是H5 #####
###### 这是H6 ######
注意：#之间建议保留一个字符空格，这个是标准的markDown语法。
# 列表的几种样式 #
## 样式一 ##
- 样式一
- 样式一
- 样式一    

  
## 样式二 ##
1. 样式二
2. 样式二
3. 样式二

注意：-、和1. 后面都要和后面的文本要保留一个空格。
# 链接和图片的格式 #
## 链接格式 ##
MarkDown语法中，插入链接是通过【文本】（网址）这样的语法格式实现的。例如：  
[百度]（www.baidu.com）
## 图片格式 ##
MarkDown语法中，插入链接是通过！【文本】（网址）这样的语法格式实现的。例如：  
！【图片名称】（图片地址）
注意：插入图片和链接的语法很相似，只是在前面多添了一个感叹号。
# 引用的格式 #
我们写作的时候，有时候经常引用别人的文字，所以我们需要加上引用格式。在MarkDown中，你只需要在你需要引用的文字前面加上> 符号就好了，例如：  
> 曾经沧海难为水，除却巫山不是云
   
注意：>和后面文本之间要保留一个空格。

# 粗体和斜体的格式 #
在MarkDwon的语法中，用两个\*包含文本就是粗体的语法，用一个\*包含文本就是斜体的语法。例如：  
**曾经沧海难为水**，*除却巫山不是云*
# 代码的引用格式 #
如果只有一段代码没有分行就可以用\’扩起来。
如果引用的代码有多行就可以三个点号`放于段首和段尾括起来,段首括起来要加一个回车或者直接按table键或四个空格表示代码块。例如：  

        geren_qu.setOnClickListener(this);
        geren_wan.setOnClickListener(this);
        geren_pop_image.setOnClickListener(this);
        image_add_time.setOnClickListener(this);
        time_end.setOnClickListener(this);
        time_start.setOnClickListener(this);
        tv_shengcheng.setOnClickListener(this);
        ct_fuyongshuoming.setOnClickListener(this);
        ct_yaowujieshao.setOnClickListener(this);
        ct_yaowumingcheng.setOnClickListener(this);
        top_itv_back.setOnClickListener(this);

# 显示连接中带括号的图片 #
![][1]
[1]: http://latex.codecogs.com/gif.latex?\prod%20\(n_{i}\)+1

```

	![][1]
	[1]: http://latex.codecogs.com/gif.latex?\prod%20\(n_{i}\)+1
```

# 换行的方法 #
你只需要在你想要换行的地方打两个空格，就会自动换行。
# 分隔符的格式 #
如果你想写分隔符，可以新起一行输入一个减号。如果前后都有段落的时候，要空出一行，例如：     
\---
# 符号转义 #

如果你不想你的秒速中用的markdown符号，比如_,#,*等，但又不想它被转义，这个时候可以在这些符号前加反斜杠，如\\\_,\\#,\\\*等进行避免。  

