---
layout: post
title: Jekyll+Github Pages 建站
comments: true
tags: Tech, Website
---

个人博客托管在Github Pages上非常方便，只需要在github上建一个repo，然后每次push代码到ghpages这个branch就能自动上传到Github Pages。对程序员来说这是一个免费可自定义而且易于维护博客的方法。花了一天的时间总算弄的有点雏形，来和大家简单分享下。

这回建立博客的框架采用了[Jekyll](https://jekyllrb.com/),这个框架产生的是纯静态的网页，不自带数据库，因此留言等带有交互性的操作就需要依靠第三方插件。

### github准备
在github上创建branch `blog`
本地创建博客目录已经生成git
```
$ mkdir blog
$ cd blog
$ git init
$ git checkout --orphan gh-pages
$ git remote add origin https://github.com/username/jekyll_demo.git
```
### Jekyll在MacOS的安装

安装ruby，因为Jekyll是基于ruby写的

```
$ brew install ruby
```

安装Jekyll

```
$ sudo gem install jekyll
```

检查是否安装成功

```
$ jekyll -v
```

其他的操作系统请参考[官网](https://jekyllrb.com/docs/installation/)
### 基本框架搭建、本地调试和上线
在根目录下创建基本框架
```
$ jekyll new .
```
在根目录下修改`config.yml`文件，加入
```
baseurl: /blog
destination: _build
```
创建模板文件夹和_`build`输出文件夹，`_build`输入文件夹是Jekyll框架build完后输出的内容，如果不加这个文件夹build完后就会在各自的文件夹下自动创建`_site/`。文件夹名字可以更改并在config.yml里改变destination参数，`_build`文件夹可以放到.gitignore文件中，不需要commit。
```
$ mkdir _layouts
$ mkdir _build
```
进入_layouts创建default.html模板，Jekyll采用的是[Liquid](https://shopify.github.io/liquid/basics/introduction/)模板
{% raw %}
```
<!DOCTYPE html>
<html>
　　<head>
　　　　<meta http-equiv="content-type" content="text/html; charset=utf-8" />
　　　　<title>{{ page.title }}</title>
　　</head>
　　<body>
        {{content}}
　　</body>
</html>
```
{% endraw %}
建立第一篇post，根目录下创建`_posts`文件夹，所有的博客都写入这个文件夹下。
```
$ mkdir _posts
$ cd _posts
```
创建文件`2018-05-12-hello-world.md`，文件名字前缀必须是年月日，Jekyll会按照输入的文件名输出关于时间结构的目录
在新创建的文件中写入如下代码。
```
---
layout: default
title: 你好，世界
---
Hello World！
```
建立主页，在根目录下建立文件`index.html`
{% raw %}
```
---
layout: default
title: 主页
---
<ul>
    {% for post in site.posts %}
    <li>{{ post.date | date: "%Y-%m-%d" }} <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>
```
{% endraw %}
现在基本代码都写好了，只需要commit后上传到github就可以了。在根目录下运行
```
$ jekyll serve --livereload
```
在浏览器中打开`http://localhost:4000/blog/index.html`

回到根目录
```
$ git add .
$ git commit -m 'Initial commit: hello world post'
$ git push origin gh-pages
```
过几分钟访问
`http://{user}.github.com/blog/`就可以看见了。

### Style
功能完成后就需要更改样式了，Jekyll自带支持[sass](https://sass-lang.com/)，样式支持
。
````
/blog
    |- _build
    |- _layouts
    |- _posts
    |- _sass
        |- post.scss
    |- css
        |- main.scss
    |- index.html
    |- _config.yml
````
首先在根目录创建`css`和`_sass`文件夹，`css`文件夹是主css的文件夹，默认会把`css`里的scss文件build到`_build/css`中, Jekyll会把`css/main.scss`当做普通的template，因此文件最上面要加两排横线。`_sass`文件夹包括了css模块化的文件，里面的文件通过`@import`加入到`_sass/main.scss`中。
`_sass/post.scss`
{% raw %}
```
h1 {
    color: red;
}
```
{% endraw %}
`css/main.scss`
{% raw %}
```
---
---
@import "post";
```   
{% endraw %}

在`_layouts/default.html`里加上css
{% raw %}
```
<link rel="stylesheet" href="{{ '/css/main.css' | relative_url }}">
```
{% endraw %}

### Markdown语法写博客
写博客时支持[markdown语法](https://guides.github.com/features/mastering-markdown/)（.md文件）和html（.html文件）的页面，Jekyll会自动把.md文件build成html文件。

本地写博客时我偏好markdown语法，因为会显得很整洁，而html会经常需要对齐看缩进。[markdown语法](https://guides.github.com/features/mastering-markdown/)也很简单都有和html一一对应的语法
使用WebStorm编写.md文件时会同步显示编译成html的页面，非常方便查看样式是否正确。

### 评论
评论有多种插件可以支持，[disqus](https://disqus.com/admin/install/platforms/jekyll/)、gitment和gitalk，我使用了disqus方法。

注册号disqus号后在body前添加下列代码。
{% raw %}
```
<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = "http://BLOG.host.com{{ page.url }}"; // <--- use canonical URL
this.page.identifier = "{{ page.id }}";
};
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');

s.src = '//SHORTNAME.disqus.com/embed.js'; // <--- use Disqus shortname

s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>
```
{% endraw %}

### 其他
```
jekyll serve --config "_config.yml,_config_dev.yml"
```