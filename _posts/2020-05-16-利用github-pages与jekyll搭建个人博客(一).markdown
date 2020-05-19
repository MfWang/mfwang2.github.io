> [利用github pages与jekyll搭建个人博客(一)](http://www.jianshu.com/p/d759ff1f1af5#)：基础环境配置与博客搭建
[利用github pages与jekyll搭建个人博客(二)](http://www.jianshu.com/p/7e68faee9512)：添加分页功能以及安装Grunt
[查看github上源码](https://github.com/MfWang/MFWang.github.io)
[github博客地址](https://mfwang.github.io/)

##第一步，配置jekyll环境
因为本人用的是mac系统，以下操作都基于此。
1.安装ruby
2.安装RubyGems
```
npm install rubygems
gem -v //2.0.14.1
```
3.安装jekyll

```
 gem install jekyll 
//报错，说
Fetching: jekyll-3.4.0.gem (100%)
ERROR:  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions for the /Library/Ruby/Gems/2.0.0 directory.
//所以之后的操作都使用sudo
sudo gem install jekyll //安装成功
jekyll -v  //查看版本
```
> * 后来帮同事安装的时候，发现`sudo gem install jekyll`有其他的报错，原因是ruby版本过低，所以要先[升级ruby](http://www.jb51.net/article/70472.htm)，然后再安装jekyll

> * 如果是之前安装好的环境，日后运行时报：
jekyll 3.4.0 | Error:  uninitialized constant Bundler::Plugin::API::Source
需要升级gem
```
sudo gem update //升级gem
```

##第二步，使用jekyll创建博客目录
```
jekyll new myblog //创建博客目录
//报错
Dependency Error: Yikes! It looks like you don't have bundler or one 
of its dependencies installed. In order to use Jekyll as currently 
configured, you'll need to install this gem. The full error message 
from Ruby is: 'cannot load such file -- bundler' If you run into trouble, 
you can find helpful resources at https://jekyllrb.com/help/!
jekyll 3.4.0 | Error:  bundler
```
![创建目录时报错.png](http://upload-images.jianshu.io/upload_images/1874069-daa8123daf98f43d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

此时需要安装bundler，总之就是缺什么，装什么
```
sudo gem install bundler
rm -rf myblog  //删除刚才生成的文件夹，重新生成
jekyll new blog
cd blog
jekyll serve //启动服务
```

![启动服务.png](http://upload-images.jianshu.io/upload_images/1874069-475792eca527971b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

此时，打开浏览器，输入localhost:4000 就可以看到我们的博客了
 当然，在启动服务的时候如果遇到以下这个错误
```
D:\GarryBlog>jekyll server
Configuration file: D:/GarryBlog/_config.yml
Configuration file: D:/GarryBlog/_config.yml
            Source: D:/GarryBlog
       Destination: D:/GarryBlog/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 0.252 seconds.
  Please add the following to your Gemfile to avoid polling for changes:
    gem 'wdm', '>= 0.1.0' if Gem.win_platform?
 Auto-regeneration: enabled for 'D:/GarryBlog'
Configuration file: D:/GarryBlog/_config.yml
jekyll 3.3.1 | Error:  Permission denied - bind(2) for 127.0.0.1:4000
```
这个错误是告诉我们4000端口被占用，解决方法是：在_config.yml文件的末尾加上port: 4040，改为4040端口即可。这样在浏览器中输入[http://127.0.0.1:4040](http://127.0.0.1:4040) 也可以看到我们的博客了。

![Blog测试页.png](http://upload-images.jianshu.io/upload_images/1874069-9db17fae30d0ed9a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##第三步，将简书上的文章打包下载，替换_posts目录中的文件
![简书_设置.png](http://upload-images.jianshu.io/upload_images/1874069-91d2fc446cc8ad32.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![简书_打包下载文章.png](http://upload-images.jianshu.io/upload_images/1874069-29a89a6158a5e3e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打包下载的文件：

![下载列表.png](http://upload-images.jianshu.io/upload_images/1874069-476c1cc13cbb0e78.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![文章目录.png](http://upload-images.jianshu.io/upload_images/1874069-e60f51bab57f6382.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

现在，将文件名更改`date-title.md`，如：`2016-10-13-图片上下居中.md`，操作结束后刷新页面，可以看到页面上已经更改为自己当前的文章列表了。

##第四步
在根目录下创建_layouts目录，并创建子文件：default.html
![_layout目录下的default文件.png](http://upload-images.jianshu.io/upload_images/1874069-a4753c8689470b20.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在根目录下创建index.html文件

![根目录下的index文件.png](http://upload-images.jianshu.io/upload_images/1874069-8ca8e7d2640e605b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

保存文件，刷新页面
但是蛮奇怪的是我的页面好像没有变动，还是这个样子，不知道是哪一步出了问题

![当前页面显示.png](http://upload-images.jianshu.io/upload_images/1874069-a6655ceeacc84260.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>上面的问题已解决：
在第二步jekyll new myblog之后会自动创建一个index.md的文件，将该文件删除或者内容清空，页面就会根据Index.html显示
在此，我选择将该文件删除，下面是index.md中的内容

```
---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: home
---

```



##第五步，在自己的github上创建博客项目
在自己的github上创建目录blog，然后克隆到本地，新切分支：
```
git clone *****
git checkout -b gh-page
```
再将之前文件夹里的内容全部拷贝到blog目录中，然后提交代码：
```
git add .
git commit -am "个人博客基础搭建"
git push origin gh-pages:gh-pages //将本地gh-pages分支推送到远端
```
> 注意，这里我使用的目录名blog与github的用户名不一致，所以需要切分支gh-pages，因为github规定只有gh-pages分支中的页面，才会生成网页文件。
此时博客地址：https://username.github.io/blog/
当然，也可以在创建目录时，将目录名与用户名一致，这样就不需要创建gh-pages分支了，直接在master中提交就可以生成网页文件了
此时博客地址：https://username.github.io/

另外：
>github上的博客可以自己创建，也可以fork别人的博客代码，进行改造，这里初次创建，先自己创建，后期fork别人的代码或者直接下一些简洁的模板，改成自己想要的样子。

ps:
> [利用github pages与jekyll搭建个人博客(一)](http://www.jianshu.com/p/d759ff1f1af5#)：基础环境配置与博客搭建
[利用github pages与jekyll搭建个人博客(二)](http://www.jianshu.com/p/7e68faee9512)：添加分页功能以及安装Grunt
[查看github上源码](https://github.com/MfWang/MFWang.github.io)
[github博客地址](https://mfwang.github.io/)
