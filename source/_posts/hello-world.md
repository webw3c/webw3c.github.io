---
title: 超详细Hexo+Github Page搭建技术博客教程【持续更新】
tags: [Hexo, Github Page, SEO]
categories: Hexo
---

## 前言
博客有第三方平台，也可以自建，比较早的有博客园、CSDN，近几年新兴的也比较多诸如：WordPress、segmentFault、简书、掘金、知乎专栏、Github Page 等等。

这次我要说的就是 Github Page + Hexo 搭建个人博客的方式！Github Page 是 Github 提供的一种免费的静态网页托管服务（所以想想免费的空间不用也挺浪费的哈哈哈），可以用来托管博客、项目官网等静态网页。支持 Jekyll、Hugo、Hexo 编译静态资源，这次我们的主角就是 Hexo 了，具体的内容下面在文章内介绍。

下面就开始吧~

## 准备环境
准备 node 和 git 环境，
首先，安装 [NodeJS](https://nodejs.org/en/)，因为 [Hexo](https://hexo.io/zh-cn/) 是基于 Node.js 驱动的一款博客框架，相比起前面提到过的 Jekyll 框架更快更简洁，因为天*朝网络被墙的原因尝试过安装 Jekyll 失败而放弃了。
然后，安装 [git](https://git-scm.com/)，一个分布式版本控制系统，用于项目的版本控制管理，作者是 Linux 之父。如果 Git 还不熟悉可以参考廖雪峰大神的 [Git](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000) 教程。

两个工具不同的平台安装方法有所不一样，可自行了解按步骤安装，这里不详述了。安装成功后打开git bash（Windowns）或者终端（Mac），下方中将统一称为命令行。
在命令行中输入相应命令验证是否成功，如果成功会有相应的版本号。

``` bash
git version
node -v
npm -v
```

![image](https://wx3.sinaimg.cn/large/889d1a94gy1fz2nmvl6kej20g907dwet.jpg)

## 安装 Hexo
如果以上环境准备好了就可以使用 npm 开始安装 Hexo 了。也可查看 [Hexo](https://hexo.io/zh-cn/) 的详细文档。
在命令行输入执行以下命令：

``` bash
npm install -g hexo-cli
```

安装 Hexo 完成后，再执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

``` bash
hexo init myBlog
cd myBlog
npm install
```

新建完成后，指定文件夹的目录如下：

``` bash
.
├── _config.yml # 网站的配置信息，您可以在此配置大部分的参数。 
├── package.json
├── scaffolds # 模版文件夹
├── source  # 资源文件夹，除 _posts 文件，其他以下划线_开头的文件或者文件夹不会被编译打包到public文件夹
|   ├── _drafts # 草稿文件
|   └── _posts # 文章Markdowm文件 
└── themes  # 主题文件夹
```

好了，如果上面的命令都没报错的话，就恭喜了，运行 hexo s 命令，其中 s 是 server 的缩写，在浏览器中输入 http://localhost:4000 回车就可以预览效果了。
``` bash
hexo s
```

以下是我本地的预览效果，更换了 next 主题的，默认不是这个主题。

![image](https://ws1.sinaimg.cn/large/889d1a94gy1fz2r6obbifj21bo0mfdjx.jpg)

至此，你本地的博客就已经搭建成功，接下来就是部署到 Github Page 了。

## 注册 Github
首先如果你还没有 Github 账号的先[注册](https://github.com)一个，具体过程如下

![image](https://ws3.sinaimg.cn/large/889d1a94gy1fz2ovhptzgj20n90rk12d.jpg)

点击 Start project 或者下面的 new repository 创建一个新的仓库

![image](https://wx2.sinaimg.cn/large/889d1a94gy1fz2p5l7j6cj20sk0h8wju.jpg)

注意点来了，Github 仅能使用一个同名仓库的代码托管一个静态站点，这个网上很多教程没说到的。

![image](https://ws3.sinaimg.cn/large/889d1a94gy1fz2po2plspj20kr0hbtbb.jpg)

然后打开仓库创建一个 index.html 文件，并随意先写点内容，比如 Hello World.

![image](https://ws2.sinaimg.cn/large/889d1a94gy1fz2q91ar2zj20s80e50va.jpg)
![image](https://ws1.sinaimg.cn/large/889d1a94gy1fz2qami35uj20dt06a74t.jpg)

这个时候打开 http://你的用户名.github.io 就可以看到你的站点啦，是不是很简单！index.html 内容只是暂时的预览效果，后面把 Hexo 的文件部署上去就可以在 http://你的用户名.github.io 看到你自己的博客啦！ 比如我的就是 http://webw3c.github.io 了。

![image](https://wx4.sinaimg.cn/large/889d1a94gy1fz2pwjzahzj20c0040mx4.jpg)

## 配置 SSH key
要使用 git 工具首先要配置一下SSH key，为部署本地博客到 Github 做准备。

打开命令行输入 cd ~/.ssh 如果没报错或者提示什么的说明就是以前生成过的，直接使用 cat ~/.ssh/id_rsa.pub 命令就是可以查看本机上的 SSH key 了。

``` bash
cat ~/.ssh/id_rsa.pub
```

![image](https://wx4.sinaimg.cn/large/889d1a94gy1fz2rym90u1j20fr03jgm6.jpg)

如果之前没有创建，则执行以下命令全局配置一下本地账户：

``` bash
git config --global user.name "用户名"
git config --global user.email "邮箱地址"
```

然后开始生成密钥 SSH key

``` bash
ssh-keygen -t rsa -C '上面的邮箱'
```

按照提示完成三次回车，即可生成 ssh key。通过查看 ~/.ssh/id_rsa.pub 文件内容，获取到你的 SSH key

![image](https://wx2.sinaimg.cn/large/889d1a94gy1fz2s3i3zi0j20iz0anq7a.jpg)

（此图引用自码云）

首次使用还需要确认并添加主机到本机SSH可信列表。若返回 Hi xxx! You've successfully authenticated, but GitHub does not provide shell access. 内容，则证明添加成功。

``` bash
ssh -T git@github.com 
```


![image](https://ws1.sinaimg.cn/large/889d1a94gy1fz2s6m2kjwj20fq01g3yg.jpg)

到这还没完，还要登录 [Github](https://github.com) 上添加刚刚生成的SSH key，按以下步骤添加：

![image](https://ws2.sinaimg.cn/large/889d1a94gy1fz2s9u4bi4j205n0as0t0.jpg)

创建一个新的 SSH key, 标题随便，key 就填刚才生成那个，确认创建，搞定！！这样在你的 SSH keys 列表里就会看到你刚刚添加的密钥。

![image](https://wx2.sinaimg.cn/large/889d1a94gy1fz2sc8liqmj20sb0c9jtg.jpg)

## 部署到 Github
此时，本地和Github的工作做得差不了，是时候把它们两个连接起来了。你也可以查看官网的[部署](https://hexo.io/zh-cn/docs/deployment)教程。
先不着急，部署之前还需要修改配置和安装部署插件。
第一：打开项目根目录下的 _config.yml 配置文件配置参数。拉到文件末尾，填上如下配置（也可同时部署到多个仓库，后面再说）：

![image](https://wx2.sinaimg.cn/large/889d1a94gy1fz2srao2z2j20f104ewey.jpg)


第二：要安装一个部署插件 [hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)。
``` bash
npm install hexo-deployer-git --save
```

最后执行以下命令就可以部署上传啦，以下 g 是 generate 缩写，d 是 deploy 缩写：

``` bash
hexo g -d
```

稍等一会，在浏览器访问网址： https://你的用户名.github.io 就会看到你的博客啦！！

## 开始写作
博客搭好了，就开始写文章了，这里简单介绍一下，详细的文档可以看 [hexo](https://hexo.io/zh-cn/) 官网。
新建文章，输入以下命令即可
``` bash
hexo new '文章标题'
```
执行完成后可以在 /source/_posts 下看到一个“文章标题.md”的文章文件啦。.md 就是 Markdown 格式的文件，具体用法可以在网上找一下，语法还是比较简单的。

在 Markdown 文章里面输入你的文章内容

![image](https://ws1.sinaimg.cn/large/889d1a94gy1fz5x2qhmzbj216e0c50us.jpg)

再执行一下以下命令

``` bash
hexo g
```
``` bash
hexo s
```

就可以看到你的文章在博客显示了，以下就是刚刚

![image](https://wx1.sinaimg.cn/large/889d1a94gy1fz5x6potcgj20vk0e10te.jpg)

最后，只要部署到你的 Github 上就可以了！

``` bash
hexo clean
```
``` bash
hexo g -d
```

部署前最好能先执行一下 hexo clean 命令，清除缓存文件 (db.json) 和已生成的静态文件 (public)。在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令。

另外，如果你的文章暂时不发布可以先保存在草稿里面。生成草稿的方法和文章差不多 hexo new draft "文章标题"，生成后会在 /source/_drafts 里看到你的草稿文章。当你想发布时只要执行 publish 命令即可发布到博客。

```
hexo publish [layout] <filename>
```

## 静态图床
文章里用的一些图片放哪里比较好呢？比对了几个免费的图床七牛、sm.ms和微博图床，最后我决定选用微博的，七牛的好像最近是[测试域名](https://developer.qiniu.com/fusion/kb/1319/test-domain-access-restriction-rules)不能用了，虽然有解决方案，但怕以后还会有其他问题，所以放弃啦，毕竟免费的东西才是最贵的，特别像云储存这种东西，感觉都是钱钱钱，哈哈哈，万一有一天不让用就比较麻烦了，另外[sm.ms](https://sm.ms/)这个口碑也不错，好像是个人开发的，免费好几年了，有同样的担心就放弃了，最好抱了新浪的大腿，感觉新浪应该会靠谱一点吧，唯一的问题就是如果有一天新浪禁止外链的话就不行了，再看吧。

可以去chrome网上应用商店下载一个叫[微博图床](https://chrome.google.com/webstore/detail/%E5%BE%AE%E5%8D%9A%E5%9B%BE%E5%BA%8A/pinjkilghdfhnkibhcangnpmcpdpmehk?hl=zh-CN)的chrome插件，下图是插件的界面，操作简单方便，具体使用看说明就可以啦，比较简单，这样图床的问题就解决了。

![image](https://ws2.sinaimg.cn/large/889d1a94gy1fz72py0lycj20nw0go40c.jpg)

## 主题配置
你可以到[官网主题页](https://hexo.io/themes/)或者网上找你喜欢的，很多都不错，我使用的是 next 主题。你可以根据主题[官网使用文档](http://theme-next.iissnan.com/getting-started.html)说明修改相应的配置，达到自己想要的效果。例如设置字体、开启打赏功能、添加评论系统、设置腾讯公益404页面、数据统计、内容分享等等功能。这里我说一下简单说一下我个人用到的一些配置。

### ** 一、添加评论系统 **
*** 注意我现在已经改用 gitalk 啦，而下面是之前写的 valine 的教程，后面有空会更新或者增加这部分相应的内容，当然，如果你想使用的是 valine 可以继续参考下面的方法配置 ***

添加 [valine](https://valine.js.org/) 评论系统，打开 /themes/next/_config.yml 文件，搜索 valine，开启 valine，设置 enable 为 true。

![image](https://ws3.sinaimg.cn/large/889d1a94gy1fz5yqhmvbqj20x009dq5c.jpg)

然后到 leanCloud [登录](https://leancloud.cn/dashboard/login.html#/signin)或者[注册](https://leancloud.cn/dashboard/login.html#/signup) 一个账号，注册后登录创建一个应用，然后进入应该到设置里面找到 AppId 和 AppKey 复制粘贴到主题配置文件里面对应的地方，就是上图中的 appid 和 appkey 这两个地方。

![image](https://wx3.sinaimg.cn/large/889d1a94gy1fz5ywov61yj20zj0ea777.jpg)

### ** 二、配置腾讯公益404页面 **

1. 在博客根目录 /source 文件夹下创建404.html（具体内容见下图及代码）；
2. 在 html 上方加入上面3行代码；

腾讯公益用到的js其实有有三个，search_children.js、data.js以及page.js，如果你的站点协议是 http 的话直接按照 next 文件的方法添加就可以了，但如果是 https 话直接添加是会报错的，因为腾讯公益404页面暂时还不支持 https，所以我直接把 page.js 的内容直接加入到页面是可以成功的，请看下图

![image](https://wx4.sinaimg.cn/large/889d1a94gy1fz5zivfhysj210n0cpq5c.jpg)

上图最前面的那几行也要加进去哦。

``` bash
layout: false
title: "404"
---
```

这里放一下上面用到的几个js链接，来自腾讯公益404官方[接入文档](https://www.qq.com/404/)

``` bash
<script type="text/plain" src="http://www.qq.com/404/search_children.js" charset="utf-8" homePageUrl="https://pojian.xyz" homePageName="回到我的博客主页"></script>
<script src="https://qzone.qq.com/gy/404/data.js" charset="utf-8"></script>
<script src="https://qzone.qq.com/gy/404/page.js" charset="utf-8"></script>
```

你也可以直接复制我 Github 上的[404页面代码](https://github.com/webw3c/webw3c.github.io/blob/master/404.html)，以下是我博客的预览效果。

![image](https://ws1.sinaimg.cn/large/889d1a94gy1fz5zpo960mj21hb0r5jxt.jpg)

另外如果需要自定义个性化404页面的只要删除相应腾讯的JS，直接修改上面的 404.html 文件就可以了。

### ** 三、添加字数统计和阅读时长 **
首先安装一个插件

``` bash
$ npm install hexo-symbols-count-time --save
```

接着博客根目录下的配置文件里添加以下配置

``` bash
# 文章字数统计
symbols_count_time:
  symbols: true
  time: true
  total_symbols: true
  total_time: true
```

最后到 next 主题的配置文件下开启 symbols_count_time 字段

![image](https://ws4.sinaimg.cn/large/889d1a94gy1fz7fsfyw5hj20rx05gt97.jpg)

重启一下 hexo 就可以看到效果啦

![image](https://ws2.sinaimg.cn/large/889d1a94gy1fz7fu1u1l8j20vq08p75j.jpg)

### ** 四、开启fancybox **
打开主题配置文件搜索 fancybox 设置为 true，另外，vendors 填上对应 CDN 地址即可开启

![image](https://wx3.sinaimg.cn/large/889d1a94gy1fz7fy76s0uj20n50cimzp.jpg)

### ** 五、文章分享 **
百度分享有个 https 的坑，按网上的方法把文件放到自己的服务器是我以前在其他的网站上测试过是能使用的，但在 hexo 中却报错了，具体不清楚是什么原因，感觉可能是 hexo 版本的问题，因为有的人可以，有的人报和我一样的错误，忘记截图了。因为这个功能也没有十分需要，就不继续爬了。换了 [share.js](https://github.com/overtrue/share.js) 实现了同样的功能，具体可以看本文文末的效果。

## 绑定域名
如果你感觉直接使用 github.io 的域名作为你的博客链接不够专业，不够程序员的话那么就购买一个域名解析绑定到你的博客，我也比较建议这样做。
我的是在阿里万网[注册](https://wanwang.aliyun.com/domain/?spm=5176.100251.111252.17.365b4f157s4Od7)的，注册流程比较常规这里就不多详述了，

注册登录控制台后找到你的域名，点右侧的解析按钮进去解析列表

![image](https://wx1.sinaimg.cn/large/889d1a94gy1fz68md161tj21hc0f3q6e.jpg)
![image](http://wx3.sinaimg.cn/large/889d1a94gy1fz68pz0zjuj21fw0dtwgt.jpg)

点右边的“添加记录”添加两条 CNAME 类型的记录，如上图，后面的记录值就填写你们自己的 Github 地址哈

记录添加完后就要到 [Github](https://www.github.com) 设置绑定你购买的域名了，进入你的博客仓库点 Setting，然后拉到 GitHub Pages 那里填上你的申请购买的域名保存就可以了

![image](https://ws1.sinaimg.cn/large/889d1a94gy1fz68xr3w2fj20sf05xgmd.jpg)
![image](https://ws4.sinaimg.cn/large/889d1a94gy1fz68zmvj8ij20l90i5gny.jpg)

这里说下，当你点击保存的时候 Github Pages 会自动帮你生成一个 CNAME 的文件在根目录，里面的内容就是你绑定的域名地址

![image](https://wx1.sinaimg.cn/large/889d1a94gy1fz694nu6n0j20tp0a4abc.jpg)

注意，如果是按上面的方法操作还会有一点小问题，就是当你执行 hexo d 部署你本地的文章到 Github 时，你本地的文件会全部覆盖掉你现有仓库上的所有内容，包括 Github Pages 帮你创建的那个 CNAME 文件，这样的话当你访问域名的时候又会访问不到了。所以呢，你需要自己手动在本地根目录 /source 目录下手动创建一个 CNAME 文件，内容就是你的域名地址，因为 source 目录下的文件部署的时候是不会被删除的，所以部署的时候也会一起被部署上去，最后还需要重新到你仓库 Setting，拉到 GitHub Pages 那里再一次绑定你的域名，这样以后就没问题了。

稍等一会就可以用你申请的域名就访问你的博客了！

## SEO优化
接下来说下百度收录，据说 Github 屏蔽了百度的蜘蛛，也有的人说没，具体不是很清楚，Github 在2015的时候遭受了史上最大规模的DDoS攻击，有国外媒体指百度干的，具体不得而知啦，但感觉百度收录 Github 确认是比较难，时间也比较长，所以还是优化一下吧。

### 一、代码同时部署到 coding
那有什么方法呢？就是把博客站点同时托管在国内的 coding 平台上，这样收录就会容易很多，同时又不影响 Github 上的代码，[coding](https://coding.net/) 是国内的一个提供代码托管服务的平台，跟 Github 差不多。使用方法也和 Github 差不多，下面我就具体说一下怎么把代码同时部署到 coding 和 Github 上面，让百度更容易收录。

[注册](https://coding.net/register)、[登录](https://coding.net/login) coding 后创建一个新的仓库，注意点就是新建项目的时候命名规则和 Github 上的一样，就是 **用户名.coding.me** 可以看下图，还有记得别忘了添加 SSH key

![image](https://ws2.sinaimg.cn/large/889d1a94gy1fz78hhe593j20qw05dmxe.jpg)

仓库建好后进入仓库，选左侧的 Page 服务，在设置中绑定新的域名，输入后点击绑定就可以了

![image](https://ws2.sinaimg.cn/large/889d1a94gy1fz78v08sssj21fn0ppgrv.jpg)

接着到你的域名解析控制台修改添加两条 CNAME 记录指向你的 Page 地址，看下图，注意看记录值哦，换成你自己的。

![image](https://wx3.sinaimg.cn/large/889d1a94gy1fz78yh3racj21ay0czgnm.jpg)

最后呢修改根目录下的 _config.yml 配置文件中的部署配置，把 coding 的 git 地址添加进去就行了

![image](https://wx3.sinaimg.cn/large/889d1a94gy1fz793dwgpfj20p005paak.jpg)

最后执行部署命令

``` bash
hexo clean
hexo g -d
```

这时就可以在 coding 仓库中看到你提交部署的代码了，同时 你的用户名.coding.me 也可以访问你的博客站点了，这里 Github 和 coding 的代码是同时更新的，互不影响。而绑定的域名解析可能需要稍等一会才会生效。

### 二、百度提交链接
部署到 coding 后也不是百度就可以收录的，我们还需要继续优化。如果在百度搜索输入 site:你的域名 如果出现以下的效果证明就是网站还没被百度收录的，我们现在点下面的[提交网址](https://ziyuan.baidu.com/linksubmit/url)，进入百度站长工具提交。

![image](https://wx4.sinaimg.cn/large/889d1a94gy1fz7a9r2nifj20w209x0ty.jpg)

### 三、百度站长平台添加网站管理

注册[百度站长](https://ziyuan.baidu.com/site/)工具，并添加网站

![image](https://wx3.sinaimg.cn/large/889d1a94gy1fz7amrsnmoj20ym0aeabo.jpg)

添加网站的过程有三步，主要操作集中在最后一步的网站验证方式里，我选择的是 HTML标签验证，按下面使用方法添加代码到你的网站即可

![image](https://ws2.sinaimg.cn/large/889d1a94gy1fz7atp67kej20s70l2tav.jpg)

而使用 next 主题的同学可以直接在主题的配置文件下搜索 baidu_site_verification 后面填上第三步中 meta 标签中 content 的值就可以

![image](https://ws1.sinaimg.cn/large/889d1a94gy1fz7aydsnr6j20rz0asn02.jpg)

最后点完成验证就可以通过了。

### 四、添加sitemap站点地图
站点地图包含了你网站上的站点链接，方便搜索引擎蜘蛛的抓取工作，搜索蜘蛛会通过网站地图中链接的深层次爬行，抓取新的内容。所以我们要生成 sitemap 文件助于网站优化，安装生成插件

``` bash
npm install hexo-generator-sitemap --save
npm install hexo-generator-baidu-sitemap --save
```

接着在博客根目录下的配置文件里添加对应配置项

``` bash
# sitemap
sitemap:
  path: sitemap.xml
baidusitemap:
  path: baidusitemap.xml
```

注意缩进，要不会编译报错

还要修改一个根目录配置文件下的URL，url 一项的值改成你在百度站长平台里面添加的自己站点的地址，这样生成的 sitemap.xml 文件里的 url 才是你站点的地址，看下图

![image](https://wx3.sinaimg.cn/large/889d1a94gy1fz7cay1xf9j20qc03wjrs.jpg)

### 五、添加蜘蛛协议robots.txt
增加 robots.txt 文件，就是蜘蛛协议，新建 robots.txt 文件添加以下内容，把 robots.txt 放在 /source 文件下，我们前面说过 /source 目录下的文件是会被打包上传的。

``` bash
# hexo robots.txt
User-agent: *
Allow: /
Allow: /archives/

Disallow: /vendors/
Disallow: /js/
Disallow: /css/
Disallow: /fonts/
Disallow: /vendors/
Disallow: /fancybox/

Sitemap: https://pojian.xyz/sitemap.xml
Sitemap: https://pojian.xyz/baidusitemap.xml
```

Allow后面的就是你的menu，还有最下面的 Sitemap 地址请自行改成你们自己的地址

完成后，重启hexo，执行 hexo g -d 重新生成文件并提交后，在public目录下会生成对应的xml文件。可以通过 http://xxx.com/sitemap.xml 和 http://xxx.com/baidusitemap.xml 访问到 sitemap 文件，通过 http://xxx.com/robots.txt 访问到 robots.txt 文件。

可以到百度站长检测一下 robots.txt 文件是否生效

![image](https://ws1.sinaimg.cn/large/889d1a94gy1fz7df4tdohj210m0pnag3.jpg)

### 六、自动推送
百度有自动推送、主动推送、sitemap、手动提交几种方式。
自动推送是轻量级链接提交组件，将自动推送的JS代码放置在站点每一个页面源代码中，当页面被访问时，页面链接会自动推送给百度，有利于新页面更快被百度发现。怎么安装呢？
如果你的是 next 主题，只要打开主题配置文件搜索找到 baidu_push 设置为 true 即可

![image](https://ws1.sinaimg.cn/large/889d1a94gy1fz7ee6cn1hj20s001hglo.jpg)

如果你使用的不是 next 主题，也可以手动把以下代码粘贴到你的站点，一般放在 head 头部公共文件里面

``` bash
<script>
(function(){
    var bp = document.createElement('script');
    var curProtocol = window.location.protocol.split(':')[0];
    if (curProtocol === 'https') {
        bp.src = 'https://zz.bdstatic.com/linksubmit/push.js';
    }
    else {
        bp.src = 'http://push.zhanzhang.baidu.com/push.js';
    }
    var s = document.getElementsByTagName("script")[0];
    s.parentNode.insertBefore(bp, s);
})();
</script>
```

### 七、主动推送
这里利用一个第三方插件 [hexo-baidu-url-submit](https://github.com/huiwang/hexo-baidu-url-submit) 进行主动推送
安装

``` bash
npm install hexo-baidu-url-submit --save
```

添加想关配置到根目录下的配置文件里

``` bash
# 百度链接提交-主动推送配置
baidu_url_submit:
  count: 3 ## 提交最新的一个链接
  host: pojian.xyz ## 在百度站长平台中注册的域名
  token: 3GIEYsuq5ZTkvDBm ## 请注意这是您的秘钥，所以请不要把博客源代码发布在公众仓库里!
  path: baidu_urls.txt ## 文本文档的地址， 新链接会保存在此文本文档里
```

添加新的部署配置，注意这里跟之前有点不一样，要在 type 前添加一个破折号 -

``` bash
deploy:
  - type: git
    repo:
      github: https://github.com/webw3c/webw3c.github.io.git
      coding: https://git.dev.tencent.com/yusting/yusting.coding.me.git
  - type: baidu_url_submitter
```

最后，执行 hexo deploy 的时候，新的连接就会被推送了。
** 实现原理 **
新链接的产生， hexo generate 会产生一个文本文件，里面包含最新的链接
新链接的提交， hexo deploy 会从上述文件中读取链接，提交至百度搜索引擎

### 八、手动推送
就是直接直接把你需要提交的链接直接使用手动的方式填写提交就可以。

最后你可以看到是否已经被百度成功收录了

![image](https://wx3.sinaimg.cn/large/889d1a94gy1fz7eumo8agj20ti0nkaez.jpg)

如果抓取成功了就证明已经被收录了，好像一般不会这么快，我的等了两天左右才抓取得到。

### 九、添加百度统计
添加[百度统计](https://tongji.baidu.com/web/homepage/index)就可以查看你网站相关的一些数据，便于你自己的站点

![image](https://ws3.sinaimg.cn/large/889d1a94gy1fz7f4xv3w1j21hc0cx79q.jpg)

![image](https://wx2.sinaimg.cn/large/889d1a94gy1fz7f64cwx8j21h00ec79e.jpg)

## 多端同步写作
内容准备中...

## 手机编写
网上好像找不到什么资料，不过通过在手机端安装 SSH 客户端远程操作服务器端，安装配置 node / git / hexo 环境编写应该可以的，原理同多台电脑编写差不多，不过这样做不太省心，不折腾了哈。

## 结语
文章到这差不多啦！后续有些小点深入学习后还是会保持更新的，希望文章对曾经像我一样的小白有那么一点帮助，技术有限，难免有纰漏，欢迎指正批评和讨论，感谢阅读！:-)