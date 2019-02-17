---
title: 2.Advanced Markdown Syntax
toc: true
date: 2018-05-13 01:21:04
tags:
 - Hexo
 - Markdown
categories:
 - Markdown
---

# Markdown语法部分
***
## 内容目录
在段落中填写 ``[TOC]`` 以显示全文内容的目录结构。

效果如下：

[TOC]

## 在markdown中使用HTML中的特殊符号

Markdown 由于其简洁的排版风格受到很多码字员的喜欢，由于其本身支持HTML语法， 所以在markdown中加入HTML支持的语法会使文章更具表现力。特殊符号表见[简书博客](https://www.jianshu.com/p/7bcf4ad609cf)或者站内链接{% post_link color-and-character "Color and Characters"%}。

## 定义型列表（暂不支持）

```
名词 1
: 定义 1（左侧有一个可见的冒号和一个不可见的空格）
```
**显示效果**

名词 1
:   定义 1（左侧有一个可见的冒号和一个不可见的空格）

代码块 2
:   这是代码块的定义（左侧有一个可见的冒号和四个不可见的空格）
***

## 站内链接
```
{% post_link 文章文件名（不要后缀） 文章标题（可选） %}
```

文章文件名和标题可加`""`，但是不能加后缀
如文章文件名为 Hello-World.md：

```
{% post_link Hello-World %}
{% post_link "Hello-World" 你好世界 %}
```

## 段落缩进（空格）
**Markdown** 天然不支持空格及段落缩进，因此需要采用插入空格**HTML**代码的形式强行实现空格和段落缩进。
```
半方大的空白&ensp;或&#8194;看，飞碟
全方大的空白&emsp;或&#8195;看，飞碟
不断行的空白格&nbsp;或&#160;看，飞碟
&emsp;&emsp;段落从此开始。
```
**显示效果**
半方大的空白&ensp;或&#8194;看，飞碟
全方大的空白&emsp;或&#8195;看，飞碟
不断行的空白格&nbsp;或&#160;看，飞碟
&emsp;&emsp;段落从此开始。
***
## 表格
使用Markdown写东西有时需要插入表格，方式有两种：

1. 使用Markdown的表格语法
2. 使用html的`<table>`标签来创建表格

### Markdown表格语法
第一行为表头，第二行分隔表头和主体部分，第三行开始每一行为一个表格行。列于列之间用管道符`|`隔开。原生方式的表格每一行的两边也要有管道符。第二行还可以为不同的列指定对齐方向。默认为左对齐，在-右边加上`:`就右对齐。
1. 简单方式
    ```
    学号|姓名|分数
    -|-|-
    小明|男|75
    小红|女|79
    小陆|男|92
    ```
    **显示效果**

学号|姓名|分数
-|-|-
小明|男|75
小红|女|79
小陆|男|92

2. 原生方式
    ```
    |学号|姓名|分数|
    |-|-|-|
    |小明|男|75|
    |小红|女|79|
    |小陆|男|92|
    ```

    **显示效果**

|学号|姓名|分数|
|-|-|-|
|小明|男|75|
|小红|女|79|
|小陆|男|92|

3. 为表格第二列指定方向
    ```
    | Tables   |      Are      |  Cool |
    |----------|:-------------:|------:|
    | col 1 is |  left-aligned | $1600 |
    | col 2 is |    centered   |   $12 |
    | col 3 is | right-aligned |    $1 |
    ```
    **显示效果**

| Tables   |      Are      |  Cool |
|----------|:-------------:|------:|
| col 1 is |  left-aligned | $1600 |
| col 2 is |    centered   |   $12 |
| col 3 is | right-aligned |    $1 |

***
### 插入table标签创建表格
```
<table>
    <tr>
        <th rowspan="2">值班人员</th>
        <th>星期一</th>
        <th>星期二</th>
        <th>星期三</th>
    </tr>
    <tr>
        <td>李强</td>
        <td>张明</td>
        <td>王平</td>
    </tr>
</table>
```
**显示效果**

<table>
    <tr>
        <th rowspan="2">值班人员</th>
        <th>星期一</th>
        <th>星期二</th>
        <th>星期三</th>
    </tr>
    <tr>
        <td>李强</td>
        <td>张明</td>
        <td>王平</td>
    </tr>
</table>

***
会发现表格前多了很多`</br>`换行：某些Markdown编辑器中使用``<table>``标签会出现表格前有空行的情况。

**解决方法：**
解决办法是将代码改为紧凑模式，修改代码如下：
```
<table><tr><th rowspan="2">值班人员</th><th>星期一</th><th>星期二</th><th>星期三</th></tr><tr><td>李强</td><td>张明</td><td>王平</td></tr></table>
```
**显示效果**
<table><tr><th rowspan="2">值班人员</th><th>星期一</th><th>星期二</th><th>星期三</th></tr><tr><td>李强</td><td>张明</td><td>王平</td></tr></table>

代码更改为紧凑模式的方法如下（Atom编辑器应该有方法做到，待查）：
* 将代码放入word文件中，ctrl+A全选，ctrl+H打开查找和替换，输入`^p`并点击全部替换。
* 有时由于缩进不是用`Tab`而是用空格，替换后会发现行内空格过多。解决方法是复制段落前的空格，输入并点击全部替换。


说明:
1. 由于使用Atom编辑器写东西再用Hexo发布到Github上，所以会有这种情况出现。如果是其他编辑器，比如简书，这个不会有，因为简书压根儿不支持``<table>``标签的表格，CSDN上是支持的，不会出现以上问题。
2. 使用哪种方式创建表格根据自己的需要而定，Markdown语法简单，但是缺点是不支持列宽度定义，表格样式定义，单元格合并等。``<table>``标签相比较而言灵活很多。
3. 很多时候直接写表格代码是很累的，比较好的方案是在Excel中编辑，再生成代码，网上搜索相应工具也有很多，比如[Tables Generator](http://www.tablesgenerator.com/)。
4. [c-xuan](http://c-xuan.com/2017/12/10/20171210001/)使用Excel自行写了个生成<table>代码的工具生成压缩的表格代码，在EXCEL中编辑好表格，生成的表格代码列宽会根据Excel中的表格列宽转成百分比，编辑后点击生成表格代码即在Excel文件的同一目录中出现Output.txt文件，将代码复制到Markdown中即可。[工具下载](/download/table2code.xlsm)
***

## 添加文件下载
你可以使用网盘如七牛等做成外链下载，也可以使用站内下载。使用网盘链接的方法不再赘述，使用站内下载的方法如下：
1. 在根目录下source文件夹创建/download文件夹，先把文件xxx.xls复制到这个文件夹。
2. 最后在xxxx.md中想引入下载链接时，只需要在xxxx.md中按照markdown的格式引入即可。

代码如下:
```
[点击下载](/download/Downloadtest.xlsx)
```
**显示效果**
[点击下载](/download/Downloadtest.xlsx)

***
## 字体、字号、颜色
Markdown在设计时定位为简单轻便的文本编辑工具，因此对复杂功能做了取舍。改变字体字号以及颜色等需要使用`<font>`标签
```
<font face="黑体">我是黑体字</font>
<font face="微软雅黑">我是微软雅黑</font>
<font face="STCAIYUN">我是华文彩云</font>
<font color=#0099ff size=12 face="黑体">黑体</font>
<font color=#00ffff>null</font>
<font color=gray size=5>gray</font>
```
**显示效果**
<font face="黑体">我是黑体字</font>
<font face="微软雅黑">我是微软雅黑</font>
<font face="STCAIYUN">我是华文彩云</font>
<font color=#0099ff size=12 face="黑体">黑体</font>
<font color=#00ffff>null</font>
<font color=gray size=5>gray</font>
***

## 背景色
由于 style 标签和标签的 style 属性都被和谐了（Really？）（这让Markdown虽然有HTML的躯体，却没有HTML的灵魂！），所以这里只能是借助 table, tr, td 等表格标签的 bgcolor 属性来实现背景色的功能。
使用示例：
```
<table><tr><td bgcolor=#7FFFD4>这里的背景色是：Aquamarine，  十六进制颜色值：#7FFFD4， rgb(127, 255, 212)</td></tr></table>
```
**显示效果**
<table><tr><td bgcolor=#7FFFD4>这里的背景色是：Aquamarine，  十六进制颜色值：#7FFFD4， rgb(127, 255, 212)</td></tr></table>

这里选出我比较喜欢的几个色号以备参考：

|Color Name|十六进制颜色值|rgb值|
|----------|:-------------:|------:|
|Aquamarine| #7FFFD4<td bgcolor=#7FFFD4>rgb(127, 255, 212)</td>
|Aqua| #00FFFF<td bgcolor=#00FFFF>rgb(0, 255, 255)</td>
|Blue| #0000FF<td bgcolor=#0000FF>	rgb(0, 0, 255)</td>
|Fuchsia| #FF00FF<td bgcolor=#FF00FF>	rgb(255, 0, 255)</td>
|Gold| #FFD700<td bgcolor=#FFD700>		rgb(255, 215, 0)</td>
|GreenYellow| #ADFF2F<td bgcolor=#ADFF2F>		rgb(173, 255, 47)</td>
|Red| #FF0000<td bgcolor=#FF0000>		rgb(255, 0, 0)</td>
|Yellow| #FFFF00<td bgcolor=#FFFF00>		rgb(255, 255, 0)</td>

***
**颜色名列表**
可以参考[w3school](http://www.w3school.com.cn/tags/html_ref_colornames.asp)，也可以参考{% post_link color-and-character "Color and Characters"%}
***

## 待办事宜 Todo 列表

使用带有 [ ] 或 [x] （未完成或已完成）项的列表语法撰写一个待办事宜列表，并且支持子列表嵌套以及混用Markdown语法，例如：
```
- [ ] **Cmd Markdown 开发**
    - [ ] 改进 Cmd 渲染算法，使用局部渲染技术提高渲染效率
    - [ ] 支持以 PDF 格式导出文稿
    - [x] 新增Todo列表功能 [语法参考](https://github.com/blog/1375-task-lists-in-gfm-issues-pulls-comments)
    - [x] 改进 LaTex 功能
        - [x] 修复 LaTex 公式渲染问题
        - [x] 新增 LaTex 公式编号功能 [语法参考](http://docs.mathjax.org/en/latest/tex.html#tex-eq-numbers)
- [ ] **七月旅行准备**
    - [ ] 准备邮轮上需要携带的物品
    - [ ] 浏览日本免税店的物品
    - [x] 购买蓝宝石公主号七月一日的船票
```

**对应显示如下待办事宜 Todo 列表：**

- [ ] **Cmd Markdown 开发**
    - [ ] 改进 Cmd 渲染算法，使用局部渲染技术提高渲染效率
    - [ ] 支持以 PDF 格式导出文稿
    - [x] 新增Todo列表功能 [语法参考](https://github.com/blog/1375-task-lists-in-gfm-issues-pulls-comments)
    - [x] 改进 LaTex 功能
        - [x] 修复 LaTex 公式渲染问题
        - [x] 新增 LaTex 公式编号功能 [语法参考](http://docs.mathjax.org/en/latest/tex.html#tex-eq-numbers)
- [ ] **七月旅行准备**
    - [ ] 准备邮轮上需要携带的物品
    - [ ] 浏览日本免税店的物品
    - [x] 购买蓝宝石公主号七月一日的船票
***
## 锚接（暂不支持）
```
[目录]\{ #index \}
跳转到[目录](#index)
```
*注意，上面 \{ 字符前若不添加 \\ 则会出现编译错误*
**显示效果**
`hexo markdown`暂不支持，后续考虑添加此**feature**
***

## 脚注（暂不支持）
```
使用 Markdown[^1]可以效率的书写文档, 直接转换成 HTML[^2], 你可以使用 Atom[^At] 编辑器进行书写。
[^1]: Markdown是一种纯文本标记语言
[^2]: HyperText Markup Language 超文本标记语言
[^At]: github是一款开源编辑器
```
**显示效果**
`hexo markdown`暂不支持，后续考虑添加此**feature**
***

# HTML代码及插件使用

## 插入HTML代码

在代码区块里面， & 、 < 和 > 会自动转成 HTML 实体，这样的方式让你非常容易使用 Markdown 插入范例用的 HTML 原始码，只需要复制贴上，剩下的 Markdown 都会帮你处理，例如：
***
## 实现文字居中
```
<center>「我们不想让封面消失。」</center>
```
**显示效果**
<center>「我们不想让封面消失。」</center>

***

## 插入div标签
```
<div class="footer">
   © 2004 Foo Corporation
</div>
```
**显示效果**

<div class="footer">
   © 2004 Foo Corporation
</div>

***
# Markdown小技巧

## 文本缩进
可以使用`Tab`键对若干行进行缩进。方法是选中若干段按`Tab`键，一次可以缩进两个英文字符大小的空格。两次缩进后即可引用代码。

## 中文输入切换
有时会出现输入法无法正常切换，通常原因是因为工作界面没有被输入法识别，解决方法是在工作界面点击鼠标左键，一般会恢复正常。

# 参考资料
> - [Markdown 语法手册 （完整整理版）](https://blog.csdn.net/witnessai1/article/details/52551362)
> - [Markdown 语法大全 包括设置字体 颜色](https://blog.csdn.net/qcx321/article/details/53780672)
> - [作业部落旗下 Cmd 在线 Markdown 编辑阅读器](https://www.zybuluo.com/mdeditor?url=https://www.zybuluo.com/static/editor/md-help.markdown)
> - [CSDN-markdown编辑器语法——字体、字号与颜色](https://blog.csdn.net/testcs_dn/article/details/45719357)
> - [CSDN-markdown编辑器语法——背景色](http://www.voidcn.com/article/p-firwkkkw-vc.html)
> - [Markdown使用table标签创建表格的问题](https://www.jianshu.com/p/7a655e5345b2)
> - 更多资料见[CSDN：Markdown博客专栏](https://blog.csdn.net/column/details/markdown.html)
