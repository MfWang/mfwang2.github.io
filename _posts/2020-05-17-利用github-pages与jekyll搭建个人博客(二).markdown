> [利用github pages与jekyll搭建个人博客(一)](http://www.jianshu.com/p/d759ff1f1af5#)：基础环境配置与博客搭建
[利用github pages与jekyll搭建个人博客(二)](http://www.jianshu.com/p/7e68faee9512)：添加分页功能以及安装Grunt
[查看github上源码](https://github.com/MfWang/MFWang.github.io)
[github博客地址](https://mfwang.github.io/)

## 2.1开启分页功能

在_posts目录下文件比较多的情况下，一页展示完全不合理，这时，就需要用到分页功能。

jekyll的分页功能很简单，在index.html文件中，将代码改写成

```
 {% for post in paginator.posts %}
    <li class="article__item">
        <a class="article__title" href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
    <span class="article__date">{{ post.date | date_to_string }} </span>
    </li>
{% endfor %}
```
保存文件，看运行结果，显示有报错的，原因是没有引入改插件

![index页面使用了分页功能，但是我引用安装该插件](http://upload-images.jianshu.io/upload_images/1874069-64e74072ea9d1368.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

首先是安装该插件：

```
sudo gem install jekyll-paginate
```

然后打开_config.yml，修改代码

```
# Gems
gems: 
 - jekyll-feed //这个是原来就有的
 - jekyll-paginate
#gems也可以这样写
#gems: [jekyll-feed, jekyll-paginate]
paginate: 10  
paginate_path: "page:num" 
```
再打开Gemfile文件

![Gemfile](http://upload-images.jianshu.io/upload_images/1874069-2f8f5902a991b43d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

将刚才安装的jekyll-paginate进行添加：
```
# If you have any plugins, put them here!
group :jekyll_plugins do
   gem "jekyll-feed", "~> 0.6"
   gem "jekyll-paginate", "~> 1.1.0"
end
```

保存文件之后重启服务

```
jekyll serve
```

只有分页显示还不够，还得有上一页下一页之类的链接跳转

在Index.html中添加代码：
```
{% if paginator.total_pages > 1 %}
  <!-- 分页代码 -->

  {% if paginator.previous_page %}
    <a href="{{ paginator.previous_page_path | prepend: site.baseurl | replace: '//', '/' }}"上一页</a>
  {% endif %}

  {% for page in (1..paginator.total_pages) %}
    {% if page == paginator.page %}
      <span class="active">{{ page }}</span>
    {% elsif page == 1 %}
      <a href="{{ '/index.html' | prepend: site.baseurl | replace: '//', '/' }}">{{ page }}</a>
    {% else %}
      <a href="{{ site.paginate_path | prepend: site.baseurl | replace: '//', '/' | replace: ':num', page }}">{{ page }}</a>
    {% endif %}
  {% endfor %}

  {% if paginator.next_page %}
    <a href="{{ paginator.next_page_path | prepend: site.baseurl | replace: '//', '/' }}">下一页</a>
  {% endif %}
{% endif %}
```

保存文件，刷新页面将会看到文章分页功能已经实现了，之后要做的就是完善分页样式了。

![分页功能已经实现](http://upload-images.jianshu.io/upload_images/1874069-60fabea60aa48318.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 2.2添加Grunt 

可参考[Grunt安装与环境配置](http://www.jianshu.com/p/a339f2dc3823)
或者跟我一步步做

1.全局安装grunt
```
npm install -g grunt-cli
```
2.进入博客目录，生成package.json
```
npm init
```
3.目录下安装grunt
```
npm install grunt --save-dev
```
4.安装需要的插件，这里根据个人需要来安装插件
```
npm install grunt-contrib-sass --save-dev
```
5.配置Gruntfile.js
在根目录下创建Gruntfile.js文件
```
/**
 * Created by wangmengfei on 16-12-21.
 *动态数据标签和ejs模板类似 <%= %>
 */

module.exports = function(grunt) {
    grunt.initConfig({
        //读取package.json文件信息
        pkg:grunt.file.readJSON('package.json'),

        sass:{
            dist:{
                options:{
                    sourceMap: false,
                    style: 'expanded'
                },
                files:[{
                    expand:true,
                    cwd:'_sass',
                    src:['*.scss','*.sass'],
                    dest:'css',
                    ext:'.css'
                }]
            }
        }

        //注解：
        //cwd: '', 指向的目录是相对的,全称Change Working Directory更改工作目录
        //src: ['**'], 指向源文件，**是一个通配符，用来匹配Grunt任何文件
        //dest: 'images', 用来输出结果任务
        //expand: true expand参数为true来启用动态扩展，涉及到多个文件处理需要开启
    });
    // 加载任务插件
    grunt.loadNpmTasks('grunt-contrib-sass');


    // 默认被执行的任务列表。
    grunt.registerTask('default',['sass']);
};
```

6.创建_sass文件夹，编写scss代码
```
//test.scss文件
@import './testb.scss';
.test{
  &__red{
    color:red;
  }
  &__green{
    color:green;
  }
}

//testb.scss文件
.test{
  &_red{
    color: red;
  }
}
```
7.保存文件，编译scss
```
grunt
```
8.在目录下可以看到编译之后的css文件，说明可以正常运行。

 ```
// 编译后的test.css文件
.testb_red {
  color: red;
}

.test__red {
  color: red;
}
.test__green {
  color: green;
}

/*# sourceMappingURL=test.css.map */

```

现在，就可以使用scss来编写css文件啦。当然，其他的插件也是一样的，需要的移步[Grunt安装与环境配置](http://www.jianshu.com/p/a339f2dc3823)，这里有更详细的插件和配置信息。

9.根目录下现在有目录node_modules，git上传的时候并不需要它，因此，在根目录下创建文件.gitignore，将目录node_modules写在.gitignore文件中

![.gitignore文件](http://upload-images.jianshu.io/upload_images/1874069-a991b1017195a993.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 2.3 grunt watch自动编译改动的文件

在完成2.2步之后，_sass目录下的文件改动后，需要`grunt`一下进行编译，然后再运行`jekyll serve`，改动后的样式才会显示到页面上，但是这样很麻烦，能不能像`jekyll serve`一样，改动之后直接编译运行呢？办法是有的，就是使用grunt的watch功能。

首先要安装这个插件
```
npm install grunt-contrib-watch --save-dev
```
安装完成之后修改Gruntfile.js文件
```
/**
 * Created by wangmengfei on 16-12-21.
 *动态数据标签和ejs模板类似 <%= %>
 */

module.exports = function(grunt) {
    grunt.initConfig({
        //读取package.json文件信息
        pkg:grunt.file.readJSON('package.json'),

        sass:{
            dist:{
                options:{
                    sourceMap: false,
                    style: 'expanded'
                },
                files:[{
                    expand:true,
                    cwd:'_sass',
                    src:['*.scss','*.sass'],
                    dest:'css',
                    ext:'.css'
                }]
            }
        },
        // watch部分为新增内容
        watch:{
            start:{
                files: ['_sass/*.scss'],
                tasks: ["sass"]
            }
        }

        //注解：
        //cwd: '', 指向的目录是相对的,全称Change Working Directory更改工作目录
        //src: ['**'], 指向源文件，**是一个通配符，用来匹配Grunt任何文件
        //dest: 'images', 用来输出结果任务
        //expand: true expand参数为true来启用动态扩展，涉及到多个文件处理需要开启
    });
    // 加载任务插件
    grunt.loadNpmTasks('grunt-contrib-sass');
    grunt.loadNpmTasks('grunt-contrib-watch');// 加载任务插件


    // 默认被执行的任务列表。
    grunt.registerTask('default',['sass']);
};
```

修改完成之后运行`grunt watch`，现在，一旦修改了_sass目录下的scss文件，就会自动编译啦，另外再运行`jekyll serve`，文件一旦有改动就会重新编译刷新网站内容啦。



> [利用github pages与jekyll搭建个人博客(一)](http://www.jianshu.com/p/d759ff1f1af5#)：基础环境配置与博客搭建
[利用github pages与jekyll搭建个人博客(二)](http://www.jianshu.com/p/7e68faee9512)：添加分页功能以及安装Grunt
[查看github上源码](https://github.com/MfWang/MFWang.github.io)
[github博客地址](https://mfwang.github.io/)
