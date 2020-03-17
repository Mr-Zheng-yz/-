## Mac上通过Hexo搭建个人博客记录

---

## 引

早就想搭建自己的博客来记录点滴了，之前学习的过程中，笔记有记在有道云笔记上的，有直接用txt文档来记录的，还有写在作业部落的。现在整理起来真是无从下手。之前也用过CSDN和微信公众号，但CSDN是在有些丑，微信公众号编辑起来又太麻烦了，愁...

有幸在Github上逛的时候，打开了JingBin大神的[博客](https://jingbin.me/)，这样清爽简洁的风格一下子吸引了我，碰巧在他的博客中还记录里Hexo搭建个人博客的文章，碰巧我手里正好有公司配的Mac，碰巧工作不忙...于是我的个人博客搭建之路就开始了。

<!-- more -->

在此记录一下我在Mac中搭建博客的。感谢[小马哥Mark](https://madongqiang2201.github.io/2016/07/21/Mac%E4%B8%8BHexo%EF%BC%8Bgithub-pages%E6%90%AD%E5%BB%BA%E9%9D%99%E6%80%81%E5%8D%9A%E5%AE%A2/)和[JingBin](https://jingbin.me/2016/11/19/Mac%E6%90%AD%E5%BB%BAHexo%E5%8D%9A%E5%AE%A2%E6%B5%81%E7%A8%8B%E8%AE%B0%E5%BD%95%EF%BC%8C%E6%8E%92%E9%9B%B7%E5%AE%8C%E6%88%90/)分享的教程和排雷经验，有些内容是直接从你们那里搬来的，忘博主见谅，如有侵权请联系我删除。

## 前期准备
### 准备工具
    1. Github账号，并创建一个置顶名字的远程仓库。
    2. Homebrew套件管理器
    3. Node，Git，Hexo
    4. NexT主题配置

### 1.Github上创建远程仓库

首先要在Github上创建一个新的指定命名的远程仓库（当然没有账号要先注册啦）

- 远程仓库命名规则：`username.github.io`

例如：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418114231720.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTM5NzQ3MQ==,size_16,color_FFFFFF,t_70)
按照如图所示的方式创建Repository，点击绿色的Create repository创建仓库就完成了。
注意不要勾选`Initalze this repository with a README` 用README初始化仓库可能会影响后面的流程。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418114115738.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTM5NzQ3MQ==,size_16,color_FFFFFF,t_70)
注意username要替换成自己Github的账户名。

### 2.使用Git Pages配置远程仓库

> 在不购买服务器的前提下，我们的网站需要挂在GitHub Pages上。GitHub Pages是面向用户、组织和项目开放的公共静态页面搭建托管服务，可用于搭建个人博客。

进入[GitHub Pages](https://pages.github.com/)里面有帮助文档，一步步做，完成后就能在浏览器打开http://username.github.io了。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418115012768.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTM5NzQ3MQ==,size_16,color_FFFFFF,t_70)
看到这个界面就说明项目站点已经配置成功了，接下来就是`Hexo`大显身手的时候了。

> 注意：如果你http://username.github.io首页就要是博客首页的话，建议初次配置选择首页，就是这样。完成后可以把index.html给删了，因为到最后你发现那是没用的，它将会给你造成干扰。

### 3.安装Node.js、Hexo

测试安装状态
```
$ node -v
 v4.2.4
$ npm -v
2.14.12
$ hexo -v
```

- 首先安装Homebrew套件管理器
> Homebrew是一款Mac OS平台下的软件包管理工具，拥有安装、卸载、更新、查看、搜索等很多实用的功能。简单的一条指令，就可以实现包管理，而不用你关心各种依赖和文件路径的情况，十分方便快捷。
> 
被小马哥安利了一波Homebrew，个人感觉是真香。

安装Homebrew需要Mac已经安装了Xcode。在有Xcode的情况下，在terminal终端输入以下命令安装：
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
就可以自动完成该工具的安装，如果这条命令失效了，可能是Homebrew更新了安装方式，可以去[这里](https://brew.sh/index_zh-cn.html)查看最新安装命令以及Homebrew的简要介绍。Homebrew安装完成之后，安装git和node.js简直简单的不要不要的。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418120303954.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTM5NzQ3MQ==,size_16,color_FFFFFF,t_70)

- 安装Git版本管理工具
终端命令：
```
bew install git
```

- 安装node.js
```
brew install node
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418120347610.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTM5NzQ3MQ==,size_16,color_FFFFFF,t_70)
现在铺垫都已经做完了，来测试一下吧:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418120646311.png)
Ok,接下来该主角登场了。

- 安装Hexo
```
npm install -g hexo
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418120743869.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTM5NzQ3MQ==,size_16,color_FFFFFF,t_70)
## 配置Hexo
Hexo的[中文文档](https://hexo.io/zh-cn/docs/index.html)

Hexo安装完成后，就可以用Hexo常用命令来配置个人博客了。

- 首先在任意位置创建一个文件夹，这个文件夹就是你存放本地博客的地方了。
- 然后在终端cd到你创建的文件夹下,然后执行以下命令：
```
    hexo init
    npm install
```
这样Hexo会在该文件夹创建本地所需的一切资源。本地博客的文件夹目录如下：
```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```
这样本地博客就搭建好了。

- 接下来测试一下本地博客：
```
   hexo s --debug  //启动hexo服务器，默认localhost:4000访问，可以看到调试信息
```
这样就开启了一个本地博客服务器，打开浏览器，在地址栏输入localhost:4000，就可以查看本地博客了，hexo默认生成了一片hello world博客。出现问题可以看terminal终端错误信息。按`control + c`关闭调试。

```
注意：以上hexo开头的命令，执行目录必须是你创建的博客文件夹目录。使用hexo s 也可以，只是没有了调试信息。
```

- 本地博客没问题之后,接下来就是把本地博客发布到github仓库。这样别人就可以访问了。

在terminal终端，将当前目录切换到你的本地博客目录，执行以下命令：
```
npm install hexo-deployer-git --save
```

安装完成之后，打开本地博客目录的_config.yml文件，编辑其中的deploy节点：

```
deploy:
  type: git
  repo: https://github.com/youlookwhat/youlookwhat.github.io.git
  branch: master
```

> 注意:字段前需加空格，hexo有严格的格式规范。

- 发布到github远程仓库

通过[Hexo常用命令](https://hexo.io/zh-cn/docs/commands)
```
    hexo clean  // clean本地项目，防止缓存
    hexo g      // 根据你编辑的md格式的博客，生成静态网页
    hexo d      // 将本地博客发布到github
```
然后，在浏览器地址栏输入username.github.io就可以访问你的博客了，别人也可以通过这个地址访问你的博客。如果错误请重复看以前流程，或参考其他文章，也可以联系[JingBin大神](https://jingbin.me)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418122226620.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTM5NzQ3MQ==,size_16,color_FFFFFF,t_70)
记录几个常用的Hexo命令:
```
hexo init       //在指定目录执行该命令，会将当前目录初始化为hexo站点，生成hexo站点所需的一切文件
hexo new “my new blog title”   //新建一篇文章。如果没有设置 layout 的话，默认使用 _config.yml 中的 
hexo new page <pagename>  //新建一个网页。生成网页后的路径会在终端中有提示
default_layout 参数代替。如果标题包含空格的话，请使用引号括起来。
hexo clean      // 清除缓存，如果对本地文件做了修改，同步到远程验证修改的效果之前，先clean，清除缓存
hexo generate   // 可以简写成hexo g 根据markdown文件生成静态文件
hexo server     // 或者简写成hexo s 启动本地hexo 服务器，默认localhost:4000可以访问
hexo deploy     // 或者简写成hexo d 将本地修改，部署到远端
hexo version    // 显示hexo版本
```

## NexT主题配置
请看[官方文档](http://theme-next.iissnan.com/getting-started.html)
官方文档很详细，我就不再赘述了。

## 优化
### 给博客添加tag和分类
使用命令新建一个博客文章时，会看到顶部有一段自动生成的文本:
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019041813501614.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTM5NzQ3MQ==,size_16,color_FFFFFF,t_70)
其中`tags`用来指定文章的标签，`categories`用来指定文章的分类。

接下来配置博客的标签页和分类页，输入如下命令：
```
hexo new page tags  //新建tags网页
```
注意tags名与_config.yml配置文件下的![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418135817965.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTM5NzQ3MQ==,size_16,color_FFFFFF,t_70)
一致。

这样就新建了tags标签网页，可以在本地站点文件下source下看到：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418140019791.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTM5NzQ3MQ==,size_16,color_FFFFFF,t_70)
其中`tags`就是我刚刚新建的网页。其他的如`about`、`categories`都是通过这种方式建立的，_posts文件夹下保存着`hexo new <文章名>`命令新建的文章。

在本地测试一下，如果没问题后就可以通过如下命令发布到github远程仓库了。
```
hexo clean
hexo g
hexo d
```

最后来看一下效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019041814051959.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTM5NzQ3MQ==,size_16,color_FFFFFF,t_70)
大功告成。

详细的配置请看[NexT的官方文档](http://theme-next.iissnan.com/theme-settings.html#author-sites)

## 总结
好了，我的第一篇真正的在自己的博客中发布的文章就正式结束了，强烈建议看到这篇文章的人还是参考[JingBin]和[小马哥]的搭建文章，两位大神写的更加权威，参考性更强。

最后希望自己不忘初心，有了自己的博客之后一定要坚持写自己学习的点点滴滴。技术没有屏障，学习没有止境。守得云开见月明...
