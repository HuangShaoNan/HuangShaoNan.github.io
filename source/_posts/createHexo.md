---
title: 使用hexo搭建个人博客
date: 2018-05-21 15:08:35
tags:
---
作为前端,前端技术日新月异，我们每天要保持学习,我们不仅要保持学习,还需要掌握一个好的,适合自己的学习方式,我发现大多数的前端都没有去记录自己的所学所用。
好记性不如烂笔头,写博客可以总结知识点,积累零碎片段,况且技术博客现在也是我们面试中可以让面试官快速了解自己的一种方式,那为什么不写?可能一部分人在一些第三方网站去记录学习，这样可能持续性不是很高，那我们就自己建设一个属于我们自己的
博客,是不是很酷^_^。

今天这篇文章会教大家使用hexo结合github搭建一个属于自己的博客,没有github的同学,自己去github上注册一个账号,个人建议作为程序员一定要有一个github,github作为程序员圈子最火爆的社区,你有什么理由不拥有？
好啦，下面就教大家怎样去搭建属于你自己的博客。


### 快速搭建
首先你的电脑要有git和node,没有的同学自行安装,安装好git和node之后，我们要在全局安装hexo，命令如下:
```
$ npm install -g hexo-cli
```

然后你在电脑新建一个空的文件夹,命名就以userName.github.io,例如:HuangShaoNan.github.io,终端进入新建文件夹下 执行命令
```
$ hexo init 
```

等待一定时间,你会看到你新建的文件夹里出现了好多初始化文件,现在这个时候一个基本的简单博客搭建成功了,你运行以下命令
```
hexo s 或 hexo s -p 4000
```

就能在 localhost:4000 中看到一个博客网站了,现在你要做的就是为你的博客网站做一些个性化的定制,打开根目录下的_config.yml 文件.修改文件内的参数,
```
 title: xxx的博客                 //博客网站的title  
 subtitle:                      //子标题  
 description: 前端,博客          //网站描述  
 author: xxx                   //网站作者  
 language: zh-Hans            //网站语言,用于本地化  
 ....                         // 等其它配置  
```
 ### 关联远程仓库
 博客搭建好之后,你需要做的是把博客发布到 github 上,首先你需要在 github 上新建一个仓库,命名和你本地新建的文件夹一样, username.githhub.io 值得注意的是,这里的 username 必须和你的github 用户名一样,大小写也要一样.然后你需要进入你的本地文件夹,关联你的远程仓库.

 ### 首先创建一个本地仓库
 ```
 $ git init
 ```
 ### 然后
 `$ git remote add origin git@github.com:username/username.github.io.git`

 和你github仓库做关联(这个地址就是你github上项目clone的地址)
 然后我们只要把代码推到远程仓库就行了,不过这里我们要有个约定,我们在 master 上发布博客,在 dev 分支上修改我们的博客内容和项目配置,也就是说我们发布博客后,master 分支上就是我们的博客的静态文件,dev 分支上的代码就是我们实际维护的博客内容

### 发布博客
发布之前我们先安装 hexo 的一键部署命令工具
#### 通过npm安装
`$ npm install hexo-deployer-git --save`
修改部署配置,在项目根目录下的_config.yml 中添加如下配置
```
deploy:
type: git
branch: master  //分支名称 此处是master主分支
repo: git@github.com:HuangShaoNan/HuangShaoNan.github.io.git  //此处记得改成你的 github 仓库的地址
```
### 好了，让我们提交你的 first commit
按照约定我们先切分支,通过命令把项目切换到 dev 分支

`$ git checkout -b dev`

然后 add 你的项目代码

`$ git add .`

你的第一个提交 commit

`$ git commit -m 'first commit blog'`

然后 push 你的代码到远程
`$ git push origin dev` //dev为你的分支

### 终于可以发布第一个博客了
前边我们已经下载好一键发布插件了,只需:

`$ hexo deploy`

然后你就可以看到一个属于你的 github.io 的网站了
----
这里要注意下:第一次发布完会在本地项目目录中生成一个public文件,下次发布时要先删除public,为了方便我们直接在npm脚本中添加一个npm命令:
在根目录的package.json中加入

```
"scripts": 
    {
        "deploy": "rm -rf public && hexo deploy"
    }
```

下次发布时 执行 npm run deploy就ok了。

到此我们的博客已经成功搭建,hexo还为我们提供了不同的主题，我们可以切换成自己喜好的主题.
### 改变你的博客网站主题
个人比较推荐的是 next ,大量的留白,让你的博客看起来简洁大方,或则你喜欢其它的主题也行,你需要在你的博客文件夹里clone下你需要的主题

`$ git clone https://github.com/iissnan/hexo-theme-next themes/next`

克隆完成后你会发现在项目目录themes目录下多了个next文件，没错 这就是我们clone下来的主题文件，themes文件下管理着我们的主题文件.
我们会发现next目下也有一个 _config.yml文件， 为了区分，我们可以起两个名字， 在根目录下的 _config.yml文件为站点配置,next目录下的为主题配置。
然后我们进入站点配置中 修改: 

`$ theme: next` // 修改主题为next

然后 hexo s 就能看到你修改的主题生效了,为了防止 clone 下来的主题文件和自己的博客文件冲突,我删除了clone 下来的 .git 文件,具体步骤为:
```
进入到next文件下 执行 ls -a 查看.git文件是否存在，存在的话执行 rm -rf .git 删除.git文件，再次 ls -a 查看
添加next文件更改, 进入到根目录下 执行 $ git add .
提交修改 $ git commit -m 'add next 主题'
push到远程 $ git push origin dev
最后别忘了发布你的博客 npm run deploy 发布完成后 你就可以在网址中访问你的userName.github.io博客了
```

当然除了更换主题外，还可以设置页面布局的方式，具体配置可以进入到next文件下的 _config.yml文件进行修改.