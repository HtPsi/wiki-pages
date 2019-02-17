---
title: 2.New features
toc: true
date: 2018-05-15 00:05:37
tags:
- Hexo
- 排序
- bootstrap
- LaTex
- Valine
- 评论
- comment
categories: Hexo

---
## New feature

### 使用icarus主题
The blog theme you may fall in love with, coming to Hexo. [仓库地址](https://github.com/ppoffice/hexo-theme-icarus)
Firsit,Go to your blog's root folder and clone Icarus into themes/icarus:
```
$ git clone https://github.com/ppoffice/hexo-theme-icarus.git themes/icarus
```
Edit your blog's `_config.yml`, change the theme field to `icarus` to enable this theme:
```
theme: icarus
```
Rename `_config.yml.example` in the theme folder to `_config.yml`.

To enable [Insight Search](https://github.com/ppoffice/hexo-theme-icarus/wiki/Search#insight-search) as your default search engine, you should also install hexo-generator-json-content from npm.
```
$ npm install -S hexo-generator-json-content
```
而后经过本地测试，主题配置成功：
![测试成功！](icarus1.JPG)

其他功能例如评论等可以参考主题[wiki](https://github.com/ppoffice/hexo-theme-icarus/wiki)。

### 实现排序功能（文章置顶）
功能实现参考了两篇技术博客：[Hexo博客添加文章置顶功能](https://sobaigu.com/hexo-post-stick-to-top.html)和[解决Hexo置顶问题](http://www.netcan666.com/2015/11/22/%E8%A7%A3%E5%86%B3Hexo%E7%BD%AE%E9%A1%B6%E9%97%AE%E9%A2%98/)。

在Hexo生成首页HTML时，将top值高的文章排在前面，达到置顶功能。

修改Hexo文件夹下的`node_modules/hexo-generator-index/lib/generator.js`，在生成文章之前进行文章top值排序。

需添加的代码：

```
posts.data = posts.data.sort(function(a, b) {
    if(a.top && b.top) { // 两篇文章top都有定义
        if(a.top == b.top) return b.date - a.date; // 若top值一样则按照文章日期降序排
        else return b.top - a.top; // 否则按照top值降序排
    }
    else if(a.top && !b.top) { // 以下是只有一篇文章top有定义，那么将有top的排在前面（这里用异或操作居然不行233）
        return -1;
    }
    else if(!a.top && b.top) {
        return 1;
    }
    else return b.date - a.date; // 都没定义按照文章日期降序排
});
```
其中涉及Javascript的比较函数：
```
cmp(var a, var b) {
    return  a - b; // 升序，降序的话就 b - a
}
```
修改完成后，只需要在`front-matter`中设置需要置顶文章的`top`值，将会根据`top`值大小来选择置顶顺序，`top`值越大越靠前。需要注意的是，这个文件不是主题的一部分，也不是`Git`管理的，备份的时候比较容易忽略。

以下是最终的`generator.js`内容

```
'use strict';
var pagination = require('hexo-pagination');
module.exports = function(locals){
  var config = this.config;
  var posts = locals.posts;
    posts.data = posts.data.sort(function(a, b) {
        if(a.top && b.top) {
            if(a.top == b.top) return b.date - a.date;
            else return b.top - a.top;
        }
        else if(a.top && !b.top) {
            return -1;
        }
        else if(!a.top && b.top) {
            return 1;
        }
        else return b.date - a.date;
    });
  var paginationDir = config.pagination_dir || 'page';
  return pagination('', posts, {
    perPage: config.index_generator.per_page,
    layout: ['index', 'archive'],
    format: paginationDir + '/%d/',
    data: {
      __index: true
    }
  });
};
```
经过测试，文章排序正常。

#### icarus主题相关问题

* banner 和 thumnail 引用本地图片首页能显示，但是点进文章内部引用出错
  * 问题描述：banner引用本地图片 如
    ```
    banner: css/images/salt-lake.jpg
    thumbnail: css/images/salt-lake.jpg
    ```
    首页能显示，但是点进文章内部无法显示
  * 解决办法：务必不要把图片放入红色的主题内部css样式文件夹中，而是把 图片放入`source`中的`images`文件夹中，并且使用路径
    ```
    /images/xxx.jpg
    ```
### Valine评论支持（待实现）

icarus主题已经支持[Valine](https://valine.js.org/)评论系统，但是主题配置的评论模块实在是不够美观，而且也不支持latex。因此我决定自己修改主题文件来自定义评论模块样式。

主要参考了官方技术文档和三篇技术博客[MonoLogueChi](https://www.xxwhite.com/2017/Valine.html)、[Deserts](https://panjunwen.com/diy-a-comment-system/)和[云淡风轻](https://ioliu.cn/2017/add-valine-comments-to-your-blog/)。另外可以参考[yilia主题](https://github.com/litten/hexo-theme-yilia/pull/646)。

### bootstrap支持（待实现）
此处挖坑：
* 可以参考hexo上不同theme中支持bootstrap技术的博客代码。
* 参考博客[cofess](http://blog.cofess.com/books/)，实现bootstrap最简单的分栏功能。
* [Bootstrap中文官网](http://www.bootcss.com/)

### LaTeX 公式

Markdown不天然支持Latex，一般通过mathjax插件来实现latex编译功能。

$ 表示行内公式：

质能守恒方程可以用一个很简洁的方程式 $E=mc^2$ 来表达。

$$ 表示整行公式：

$$\sum_{i=1}^n a_i=0$$

$$f(x_1,x_x,\ldots,x_n) = x_1^2 + x_2^2 + \cdots + x_n^2 $$

$$\sum^{j-1}_{k=0}{\widehat{\gamma}_{kj} z_k}$$

访问 [MathJax](http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference) 参考更多使用方法。

## 参考资料
> - []()
> - []()
