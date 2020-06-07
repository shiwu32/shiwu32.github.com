---
title: TD02-Markdown基础语法
date: 2019-09-01 16:35:20
top: 1
tags:
    - Markdown
categories:
    - Markdown   
---

Markdown基础语法：
通过Typora软件写md博文
<!--more-->


标题
```
# h1
## h2
### h3
#### h4
##### h5
###### h6
```
效果：
# h1
## h2
### h3
#### h4
##### h5
###### h6
```
这是一级标题
===
这是二级标题
---
```
效果：
这是一级标题
===
这是二级标题
---
```
> 这段文字将被高亮显示...
```
> 这段文字将被高亮显示...

```
插入链接或图片
[点击跳转至百度](http://www.baidu.com)

![Alt text](https://www.baidu.com/img/bd_logo1.png?where=super "百度logo")
注：
Alt text为如果图片无法显示时显示的文字；
"百度logo"为显示标题。显示效果为在你将鼠标放到图片上后，会显示一个小框提示，提示的内容就是 Optional title里的内容。
```
[点击跳转至百度](http://www.baidu.com)
![图片](https://www.baidu.com/img/bd_logo1.png?where=super)

> Markdown 格式的兼容扩展性颇佳，使之能快速转换为各种互联网上的常用格式，比如 HTML、Word、PDF 等。
  所以要实现图文混排，我们可以使用HTML语法来完成，比如完成一个图片并列排放效果，\
  就可以在MarkDown文档中直接写入如下示例代码实现当屏幕容得下多张图片时，图片将并排展示，\
  如果屏幕分辨率小，则接着下一屏展示。
```
<div style="float:left;border:solid 1px 000;margin:2px;"><img src="https://www.baidu.com/img/bd_logo1.png?where=super" ></div>
<div style="float:none;clear:both;"></div>
<div style="float:left;border:solid 1px 000;margin:2px;"><img src="https://www.baidu.com/img/bd_logo1.png?where=super"  width="200" height="260" ></div>
<div style="float:left;border:solid 1px 000;margin:2px;"><img src="https://www.baidu.com/img/bd_logo1.png?where=super" width="200" height="260" ></div>
<div style="float:none;clear:both;">
```
> 如上文，当添加了DIV样式后，图片是实现并排了，可后面的文字也贴上去了，无法实现在下一行显示，\
  此时我们就需要在将下文放在DIV容器中，并清除之前的浮动样式
```
<div style="float:none;clear:both;">
下文其他内容
</div>
```
效果如下：
<div style="float:left;border:solid 1px 000;margin:2px;"><img src="https://www.baidu.com/img/bd_logo1.png?where=super" ></div>
<div style="float:none;clear:both;"></div>
<div style="float:left;border:solid 1px 000;margin:2px;"><img src="https://www.baidu.com/img/bd_logo1.png?where=super"  width="200" height="260" ></div>
<div style="float:left;border:solid 1px 000;margin:2px;"><img src="https://www.baidu.com/img/bd_logo1.png?where=super" width="200" height="260" ></div>
<div style="float:none;clear:both;">
如上文，当添加了DIV样式后，图片是实现并排了，可后面的文字也贴上去了，无法实现在下一行显示，此时我们就需要在将下文放在DIV容器中，并清除之前的浮动样式
</div>

<div style="float:none;clear:both;">
下文其他内容
</div>




列表
```
Markdown支持有序列表和无序列表两种形式：
* 黄瓜
* 玉米
* 茄子

+ 黄瓜
+ 玉米
+ 茄子

- 黄瓜
- 玉米
- 茄子

1. 黄瓜
2. 玉米
3. 茄子
```
* 黄瓜
* 玉米
* 茄子

+ 黄瓜
+ 玉米
+ 茄子

- 黄瓜
- 玉米
- 茄子

1. 黄瓜
2. 玉米
3. 茄子

如果在单一列表项中包含了多个段落，为了保证渲染正常，* 与段落首字母之间必须保留四个空格
```
*    段落一

     小段一
*    段落二

     小段二
```
*    段落一

     小段一
*    段落二

     小段二

另外，如果在列表中加入了区块引用，区域引用标记符也需要缩进4个空格
```
* 段落一
    > 区块标记一
* 段落二
    > 区块标记二
```
* 段落一
    > 区块标记一
* 段落二
    > 区块标记二

分隔线
```
***
---
```
11111
***
11111

---
11111

插入代码块
方法是，使用反引号 进行包裹即可。如果是行内代码引用，使用单个反引号进行包裹
```
if [ -f file ]
then
    echo ok
fi
```

插入表格
```
表头|条目一|条目二
:---:|:---:|:---:
项目|项目一|项目二
```

表头|条目一|条目二
:---:|:---:|:---:
项目|项目一|项目二


> 注：三个短斜杠左右的冒号用于控制对齐方式，只放置左边冒号表示文字居左，\
  只放置右边冒号表示文字居右，如果两边都放置冒号表示文字居中。
