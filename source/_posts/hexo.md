---
title: 1.那些搭建hexo博客碰到的坑
toc: true
date: 2018-05-11 20:30:14
tags: Hexo
categories: Hexo
---
首次搭建<font color=#0000FF>hexo</font>博客是在去年八月份（感谢github提供静态网页服务），动机也很简单就是看到[xiao's group](http://www.phy.pku.edu.cn/~yfxiao/)的官方网页（那时他们的网页应该还是[hexo](https://hexo.io/)驱动的）。后来又扒拉到了[Gerard 't Hooft](http://www.staff.science.uu.nl/~hooft101/)和[Kuba Zakrzewski](http://chaos.if.uj.edu.pl/~kuba/)的个人主页，发现老一辈的物理学者都有一个朴素的页面来介绍自己的研究，所以就想着自己能亲手做一个网站作为礼物送给老刘。

在网上经过各种调查，发现已经有现成的很多轮子了（gihub上也有自己的网页项目），其中hexo算是国人用的比较多的，所以就选择了它来做自己的主页，用来试水。中间经过各种折腾，也算了解了好多互联网技术。从去年到现在网页换了两次主题，一次地址，也碰到了很多的坑，现在就总结一下。

由于时间跨度较长，很多细节都记不太清了，那就从零开始搭建一个新的博客看看会碰到什么坑吧！

## hexo建站

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。关于前期配置及相关问题可以参考[官方文档](https://hexo.io/zh-cn/docs/)。

### 配置环境

安装 Hexo 相当简单。然而在安装前，您必须检查电脑中是否已安装下列应用程序：

* [Node.js](https://nodejs.org/en/)
* [Git](https://git-scm.com/)

如果您的电脑中已经安装上述必备程序，那么恭喜您！接下来只需要使用 npm 即可完成 Hexo 的安装。

```
$ npm install -g hexo-cli
```

### 建站

安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

```
$ hexo init <folder>
$ cd <folder>
$ npm install
```
也可以直接在文件夹中直接用命令
```
$ hexo init
$ npm install
```
此处会出现如下警告：
```
`-- serve-static@1.13.2
  +-- encodeurl@1.0.2
  +-- escape-html@1.0.3
  `-- send@0.16.2
    +-- debug@2.6.9
    +-- destroy@1.0.4
    +-- etag@1.8.1
    +-- fresh@0.5.2
    +-- http-errors@1.6.3
    | +-- setprototypeof@1.1.0
    | `-- statuses@1.5.0
    +-- mime@1.4.1
    +-- range-parser@1.2.0
    `-- statuses@1.4.0

npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@^1.0.0 (node_modules\chokidar\node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.4: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})
INFO  Start blogging with Hexo!
```
根据查找[资料](https://blog.csdn.net/aerchi/article/details/54582891)，此处**warning**是因为npm引用了mac osx系统的fsevent，你是在win或者Linux下使用了，所以会有警告，忽略即可。

### 本地测试

Hexo 3.0 把服务器独立成了个别模块，您必须先安装 hexo-server 才能使用。
```
$ npm install hexo-server --save
```
安装完成后，输入以下命令以启动服务器，您的网站会在 `http://localhost:4000` 下启动。在服务器启动期间，Hexo 会监视文件变动并自动更新，您无须重启服务器。
```
$ hexo server
```
但是为了服务器稳定工作，一般我们采用静态模式，在静态模式下，服务器只处理 public 文件夹内的文件，而不会处理文件变动，在执行时，您应该先自行执行 `hexo generate`。两个命令可以连写为：
```
$ hexo s -g
```

输入此命令后我们可以看到
```
$ hexo s -g
INFO  Start processing
INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
```
打开测试页面
![本地测试](webtest.JPG)

此时本地测试已经成功，可以进行下一步部署到github上。

## 本地测试出现问题及其原因

### 文章路径出现乱码问题
此问题基本都是由于`.md`文件命名不规则引起的，特别注意的是命名不要出现空格（虽然带有空格的`.md`文件仍可编译）。

### 图片引用路径问题

如果直接使用绝对路径在`source`根目录下`/images/xxx.jpg`则不会有任何问题。但是为了管理方便，一般我们会启用配置文件中的`post_asset_folder`功能。在博客的_config.yml配置中将post_asset_folder置位true。

```
post_asset_folder: true
```

这样就可以`hexo new`新建文章时建立一个相关的文件夹存储图片（*或许还可以存储音频和视频？*）。此时可以使用相对路径`xxx.jpg`直接饮用图片。但是一般会出现首页引用路径错误，图片无法显示的问题（点进文章内部可以正常显示）。

在hexo官方文档中已经有此类问题的[解决方案](https://hexo.io/zh-cn/docs/asset-folders.html),但是当对图片样式进行复杂设计比如添加`style`属性时，内置插件似乎并不支持。在网上找到其他解决方案（[技术博客地址](http://etrd.org/2017/01/23/hexo%E4%B8%AD%E5%AE%8C%E7%BE%8E%E6%8F%92%E5%85%A5%E6%9C%AC%E5%9C%B0%E5%9B%BE%E7%89%87/)），可以通过安装插件`hexo-asset-image`的方法来解决，参见[github仓库](https://github.com/CodeFalling/hexo-asset-image)。

```
npm install hexo-asset-image --save
```
此时可以直接引用关联文件夹中的图片的相对地址`xxx.jpg`，而不是绝对地址`/images/xxx.jpg`，如：

```
![logo](logo.png)
```
**显示效果为**

![logo](logo.png)

### 本地服务不正常主要原因有：
 - 文章内容出错，如插入`hexo-markdown`不支持的格式,会爆出如下错误。
   ```
   $ hexo s -g
   INFO  Start processing
   FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
   Template render error: Error: expected end of comment, got end of file
    at Object._prettifyError (E:\Web\WebPage\node_modules\nunjucks\src\lib.js:35:11)
    at Template.render (E:\Web\WebPage\node_modules\nunjucks\src\environment.js:526:21)
    at Environment.renderString (E:\Web\WebPage\node_modules\nunjucks\src\environment.js:364:17)
    at Promise (E:\Web\WebPage\node_modules\hexo\lib\extend\tag.js:66:9)
    at Promise._execute (E:\Web\WebPage\node_modules\bluebird\js\release\debuggability.js:303:9)
    at Promise._resolveFromExecutor (E:\Web\WebPage\node_modules\bluebird\js\release\promise.js:483:18)
    at new Promise (E:\Web\WebPage\node_modules\bluebird\js\release\promise.js:79:10)
    at Tag.render (E:\Web\WebPage\node_modules\hexo\lib\extend\tag.js:64:10)
    at Object.tagFilter [as onRenderEnd] (E:\Web\WebPage\node_modules\hexo\lib\hexo\post.js:230:16)
    at Promise.then.then.result (E:\Web\WebPage\node_modules\hexo\lib\hexo\render.js:65:19)
    at tryCatcher (E:\Web\WebPage\node_modules\bluebird\js\release\util.js:16:23)
    at Promise._settlePromiseFromHandler (E:\Web\WebPage\node_modules\bluebird\js\release\promise.js:512:31)
    at Promise._settlePromise (E:\Web\WebPage\node_modules\bluebird\js\release\promise.js:569:18)
    at Promise._settlePromise0 (E:\Web\WebPage\node_modules\bluebird\js\release\promise.js:614:10)
    at Promise._settlePromises (E:\Web\WebPage\node_modules\bluebird\js\release\promise.js:693:18)
    at Async._drainQueue (E:\Web\WebPage\node_modules\bluebird\js\release\async.js:133:16)
    at Async._drainQueues (E:\Web\WebPage\node_modules\bluebird\js\release\async.js:143:10)
    at Immediate.Async.drainQueues (E:\Web\WebPage\node_modules\bluebird\js\release\async.js:17:14)
    at runCallback (timers.js:672:20)
    at tryOnImmediate (timers.js:645:5)
    at processImmediate [as _immediateCallback] (timers.js:617:5)

   ```
   - 解决方法：排查文章，但**不要将文章放入草稿文件夹`_drafts`中，因为草稿文件夹虽然最后没有显示，但依然会被编译从而产生错误!** 正确方式是将文章转移出根目录，然后再一个个拖进来编译。（或许可以更改后缀，去掉`.md`?）

 - 没有安装足够的插件：有些主题需要安装相应插件，有些则不需要。请按作者说明文档或者`github仓库issues`中解决方案操作。
 - 没有使用hexo clean命令，之前public文件夹中有残留。**此时能够正常编译，但是编译后可能出现网页显示紊乱。**
 - 胡乱安装插件，例如`icuras`主题使用了`valine`，但是在`package.json`中并没有valine标签(此处存疑，或许胡乱安装插件并不会出现编译错误)。

**如果本地服务正常，那么部署到github也因该是正常的。**

## 部署到github仓库

一般来说上传之前需要安装一个部署插件[hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)。
```
$ npm install hexo-deployer-git --save
```
然后修改配置
```
deploy:
  type: git
  repo: <repository url>
  branch: [branch]
```
不然会出现如下错误
```
INFO  Generated: fancybox/helpers/jquery.fancybox-media.js
INFO  Generated: css/fonts/fontawesome-webfont.woff
INFO  Generated: fancybox/helpers/jquery.fancybox-thumbs.js
INFO  Generated: fancybox/helpers/jquery.fancybox-buttons.js
INFO  Generated: css/fonts/fontawesome-webfont.eot
INFO  Generated: css/style.css
INFO  Generated: archives/2018/05/index.html
INFO  Generated: css/fonts/fontawesome-webfont.svg
INFO  Generated: css/fonts/fontawesome-webfont.ttf
INFO  Generated: fancybox/helpers/fancybox_buttons.png
INFO  Generated: css/images/banner.jpg
INFO  Generated: 2018/05/17/hello-world/index.html
INFO  28 files generated in 1.13 s
ERROR Deployer not found: git
```
安装完成后继续部署，一般会出现如下错误：
```
bash: /dev/tty: No such device or address
error: failed to execute prompt script (exit code 1)
fatal: could not read Username for 'https://github.com': No error
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Error: fatal: HttpRequestException encountered.
   ��������ʱ������
bash: /dev/tty: No such device or address
error: failed to execute prompt script (exit code 1)
fatal: could not read Username for 'https://github.com': No error

    at ChildProcess.<anonymous> (E:\Web\webtest2\node_modules\hexo-util\lib\spawn.js:37:17)
    at emitTwo (events.js:106:13)
    at ChildProcess.emit (events.js:191:7)
    at ChildProcess.cp.emit (E:\Web\webtest2\node_modules\cross-spawn\lib\enoent.js:40:29)
    at maybeClose (internal/child_process.js:891:16)
    at Process.ChildProcess._handle.onexit (internal/child_process.js:226:5)
```
出现此类错误原因不明，但是解决办法一般是更改仓库地址，从`HTTPS`切换到`SSH`，而后继续部署。
```
$ hexo d -g
INFO  Start processing
INFO  Files loaded in 202 ms
INFO  0 files generated in 252 ms
INFO  Deploying: git
INFO  Clearing .deploy_git folder...
INFO  Copying files from public folder...
INFO  Copying files from extend dirs...
On branch master
nothing to commit, working tree clean
Branch master set up to track remote branch master from git@github.com:xxx/webtest.git.
To github.com:xxx/webtest.git
 * [new branch]      HEAD -> master
INFO  Deploy done: git
```
我们会发现部署已经成功！部署不成功的原因一般为没有配置好SSH，关于配置SSH此处不再赘述，可以参考[技术博客](https://www.cnblogs.com/xsilence/p/6001938.html)。

## 部署后显示不正确的原因主要有：
 - **github** 只提供以`username`命名的仓库的静态网页服务，如果部署到其他仓库会只会显示文字。
   - 解决方法：使用custom domain解析到相关地址部署。
   - ~~以上为猜测，推测也可以在hexo根目录配置文件`_config.yml`中更改相关配置到仓库目录解决。~~
     ```
     url: https://username.github.io/repo/
     root: /repo/
     ```
 - github静态网页渲染速度很慢，部署到服务器后需要时间渲染。
   - 解决方法：请耐心等待,经常需要半个小时以上的时间才可以完成渲染。
 - 浏览器内存占用，包括css样式已经域名解析等。通过换浏览器能够正常显示。
   - 解决方法：清理浏览器内存



## 参考资料
> - [Hexo博客常用插件及用法](http://blog.cofess.com/2017/08/16/comon-plug-in-and-usage-of-hexo-blog.html)
> - []()
