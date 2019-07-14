# 第三章 模板
模板就是html文件，只是其中加入了模板语法，需要服务器的渲染，才能正常显示数据
## 1 模板
1. 之前我们通过render可以返回一个html页面，不过那都是固定的页面，固定的数据，但是如果数据是不确定的，是会不断改变的，该怎么做呢？
2. 是否可以先把页面写好，然后预留出固定的位置，在需要的时候再填入数据即可？

### 1.1 配置路径
在 application 中配置模板文件和静态文件的路径：
```
app = tornado.web.Application(
    template_path = 'templates', # 模板路径
)
```

### 1.2 模板调用案例
模板使用后台代码示例
```
class TemplatesHandler(tornado.web.RequestHandler):
    def get(self):
        self.render('login.html')

    def post(self):
        user = self.get_argument('name',None)
        passwd = self.get_argument('password',None)
        self.render('templates.html',username = user,passwd = passwd)

app = tornado.web.Application(
    handlers = [
        (r'/temp',TemplatesHandler),
        (r'/login',LoginHandler),

    ],
    template_path = 'templates',
)

```
模板login.html代码
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Tornado</title>
</head>
<body>
 <form method="post" action="/temp" >
    <p>用户名<br>
        <input type="text" name="name">
    </p>
     <p>密码<br>
         <input type="password" name="password">
     </p>
     <input type="submit">
 </form>
</body>
</html>
```
模板templates.html代码
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

欢迎《 {{ username }} 》登录
<br>
密码: {{ passwd }}
</body>
</html>
```

## 2 模板语法
既然有了模板，那模板的语法规则是怎样的呢？
### 2.1 模板符号
`{{ expression }}`用 {{ expression }} 中间是任何 python 表达式，或者是一个变量
示例
test.py代码
```
class TemplatesHandler(tornado.web.RequestHandler):
    def get(self):
        import time  #导入时间模块
        self.render('templates.html',time =time) # 插入调用时间模块time=time
```
templates.html代码
```
{{ 1 + 1 }}  <!--  可以是 python中的一个表达式或者一个变量-->
{{ time.time() }}  <!--  时间模块调用-->
```

`{%  directives %}`其他的模板指令
示例
```
{% if 1==2 %}<br> <!-- 判断语句-->
    判断结果成立

{% else %}<br>
    判断结果不成立

{% end %}
```

`{#  … #}`在模板中要注释python表达式的运行，需要使用这个模板语法
示例
```
{# time.time()#} <!--注释模板方法-->
```

`{{!   {%!   {#!`如果不想执行内容，需要在页面上打印出模板符号，只需要加上感叹号( ! )即可
示例
```
{{! 1 + 1}}

{%! if 1 %}     this is if {%! end %}

{#! time.time() #}}
```

### 2.2 控制语句 if判断

在模板中可以使用 `if` 判断,最后需要以 `{% end %}` 结尾

#### 2.2.1 if判断示例
test.py代码
```
class TemplatesHandler(tornado.web.RequestHandler):
    def get(self):
        self.render('login.html')

    def post(self):
        user = self.get_argument('name','no')
        self.render('templates.html',user = user,)
```
login.html代码
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Tornado</title>
</head>
<body>
 <form method="post" action="/temp" >
    <p>用户名<br>
        <input type="text" name="name">
    </p>
     <p>密码<br>
         <input type="password" name="password">
     </p>
     <input type="submit">
 </form>
</body>
</html>
```
templates.html代码
```
{% if user == '' %}
    未登录

{% else%}
    欢迎 {{ user}} 登录

{% end %}

```
### 2.3 循环语句 for and while
#### 2.3.1 for循环
在`tornado` 模板中可以使用 for 循环,最后需要以 `{% end %}`结尾

test.py代码
```
class TemplatesHandler(tornado.web.RequestHandler):
    def get(self):
        # self.render('login.html')
        urllist = [
            ('http://www.baidu.com','百度一下'),
            ('http://www.zhihu.com','知乎'),
            ('http://www.sina.com.cn','新浪'),
        ]
        self.render('templates.html', list=urllist)
```

templates.html代码
```
{% for i in list %}

    <a href="{{ i[0] }}" target="_blank">{{ i[1] }}</a><br>

{% end%}
```

#### 4.2.3.2 while循环
在 tornado 模板中可以使用 `while` 循环,最后需要以 `{% end %}` 结尾

test.py代码
```

```

templates.html代码
```
{% set a = 0%}  # 模板中定义变量需要加set

    {% while a<5 %}
        {{ a }}<br>
        {% set a += 1 %}

{% end %}
```

### 4.2.4 模板语法总结
1. `{{ … }}`:此符号中放入任意 python 表达式，或者模板中的变量
2. `{% … %} {% end %}`:此符号中放入模板中的命令，比如`if 、for`和`while`等需要注意的是，使用`if`等命令是，需要加上`{% end%}`除此之外，异常处理`try`也可以在模板中使用，但是这样做会让模板变得像 python 模块一样，因此并不建议大家这么做
3. `{# #}`:模板中的注释语句，可以让模板中指令不执行

### 4.2.5 模板符号总结
1. 必须掌握:`{{、{%、{#`三个符号的作用
2. 必须掌握:`{% if %} … {% end %}`的用法
3. 必须掌握:`{% for … %} … {% end %}{% while … %} … {% end %}`的用法

## 4.3 模板转义
通过模板，可以把数据动态的添加到页面，如果添加的值是一段html代码的话，会不会被执行呢？

### 4.3.1 转义
* 页面并没有解析，只是当作一个字符串，直接在页面上打印出来
* tornado默认是自动的转义，传入的数据都会当作字符串，不会被浏览器解析

test.py代码
```
class TemplatesHandler(tornado.web.RequestHandler):
    def get(self):
        atga = "<a href='https://www.baidu.com'target='_blank'>___百度___</a><br>"
        self.render('templates.html', atga= atga)
```
templates.html代码
```
{{atga}}<br>  <!-- 转义-->
```


### 4.3.2 raw 局部去掉转义
raw 可以自模板中去掉转义，让 tornado 在渲染的时候不去转义变量

test.py代码
```
class TemplatesHandler(tornado.web.RequestHandler):
    def get(self):
        atga = "<a href='https://www.baidu.com'target='_blank'>___百度___</a><br>"
        self.render('templates.html', atga= atga)
```

templates.html代码
```
{% raw atga %} <!-- 去掉转义-->
```

### 4.3.3  escape 局部开启转义
在开启模板不转义之后，可以使用 escape 来添加转义

test.py代码
```
class TemplatesHandler(tornado.web.RequestHandler):
    def get(self):
        atga = "<a href='https://www.baidu.com'target='_blank'>___百度___</a><br>"
        self.render('templates.html', atga= atga)
```
templates.html代码
```
{{atga}}<br> 
{{atga}}<br>
{{ escape(atga)}} # 开启某一条局部转义
```

### 4.3.4 模板去掉转义
`autoescape None`在模板中添加上面代码之后，当前模板不再转义
```
<!DOCTYPE html>
{% autoescape None%} <!--单个页面全局取消转义--->
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
{{atga}}<br> 
{{atga}}<br>
{{atga}}<br>
</body>
</html>
```

### 4.3.5 全局去掉转义
在 Application 中添加如下配置：
```
autoescape=None,
```
`autoescape`去掉整个项目的转义，配置之后，整个项目中的模板不再转义

### 4.3.6 模板转义总结
1. 局部去转义 必须掌握:raw的用法和作用
2. 模板去转义 必须掌握:模板去转义的方法和escape作用和用法
3. 全局去转义 必须掌握:Tornado全局去转义的方法

* 局部:`raw`在局部去转义，使用之后不会转义
* 模块:`escape`、`{% autoescape None %}`在模板中去转义，整个模板中默认不在转义
在不转义之后，可以使用 escape 转义
* 全局:`autoescape=None`在 Application 中添加此配置项，则整个项目不再转义


## 4.4 静态文件引用
模板中如何引用图片等静态文件呢？


### 4.4.1 配置静态文件路径
Application
```
app = tornado.web.Application(
    static_path = 'static',
)
```

### 4.4.2 两种引入方式
```
<img src="static/images/c.jpg" width="250" height="250">
<img src="{{ static_url('images/c.jpg') }}" width="250" height="250"> 
```

### 4.4.3 自动查找
在 Tornado 模板中，static 是个关键词，能够自动替换成 static_path 后的内容

### 4.4.4 添加版本号
`<img src="{{ static_url('images/2.jpg') }}" width="250" height="250"> `
使用此方法时，Tornado会自动地给静态文件添加版本号，如果版本号更改了，浏览器会自动的缓存新的静态文件`/static/images/2.jpg?v=c8b24e1f6f36c65833b9fade57f5d7b7`

### 4.4.5 静态文件的引用
必须掌握:tornado中静态文件来那两种引入方式

## 4.5 模板继承
在浏览网页的时候，可以发现页面上很多部分其实是一样的，那这些部分是在每个页面都写一次吗？

### 4.5.1 父模板
1. extends:`{% extends *filename* %}`继承模板，在子模板中会把父模板的所有内容都继承到子模板中，减少大量重复代码
2. block:`{% block *name* %}...{% end %}被词语句包裹的代码块在子模板中可以被重写，覆盖父模板中的

base.html代码
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}Templates {% end %}</title>
    <link rel="shortcut icon" href="{{ static_url('images/favicon.ico')}}"
</head>
<body>
{% block content%}
变异内容
{% end %}
</body>
</html>
```

### 4.5.2 子模板
subtemplate.html代码
```
{% extends ./base.html %} #继承父模板

{% block title%} subtemplate {% end %} # 头部标题

{% block content %}
    继承后自定义内容<br>
    <img src = "{{ static_url('images/2.jpg') }}" width="400px">
{% end %}
```

#### 4.5.2.1 子模板文件
include.html代码
```
<br>
this is tornado templates include
<br>
```

#### 4.5.2.3 子模板include引用
`{% include *filename*%}`include 可以导入一些其他的模板文件，一般使用include 的时候，模板文件中不使用block块 

subtemplate.html代码
```
{% extends ./base.html %} #继承父模板

{% block title%} subtemplate {% end %} # 头部标题

{% block content %}
    继承后自定义内容<br>

    # 子模板引入子模板
{% include ./include.html %}
{% end %}
```

### 4.5.3 模板继承总结
1. 必须掌握:`extends` 的用法和作用
2. 必须掌握:`block` 的用法和作用
3. 必须掌握:`include` 的用法和作用

```
模板继承：
    {% extends ./base.html %} #继承模板
    {% block title%} ... {% end %}#模板变异并给需要变异的定义一个名称,必须以{% end %}结束
    {% include ./include.html %}# 在一个继承模板内添加子页面
```

## 4.6 函数和类导入
通过前面我们了解到模板可以传入函数和类，那具体有哪些方法可以做到呢？
### 4.6.1 渲染时导入
在 Handler 中渲染模板时传入

test.py代码
```
class Calculation:
    def sum(self,a,b):
        return a + b

class TemplatesHandler(tornado.web.RequestHandler):
    def haha(self):
        return 'this is hahaha'

    def get(self):
        self.render('subtemplate.html',haha = self.haha, cal = Calculation) #传入
```

subtemplate.html代码
```
{% extends ./base.html %} #继承父模板

{% block title%} 测试 {% end %} # 头部标题

{% block content %}
   {{ haha() }} # 调用传入的函数
   {{ cal().sum(5,5) }} #调用传入的类的函数
{% end %}
```
### 4.6.2 模板中直接导入
在模板中也可以直接导入 python 模块

#### 4.6.2.1 内置模块导入模块
subtemplate.html 代码
```
{% extends ./base.html %} #继承父模板

{% block title%} 测试 {% end %} # 头部标题

{% block content %}
#导入内置模块与使用方法
    {% import time %}
    {{ time.time() }}
{% end %}
```

#### 4.6.2.2 自定义模块导入
注意：python解释器查找 mod_file 时是根据 py 文件的路径来查找的，不是根据模板的路径来查找

subtemplate.html代码
```
{% extends ./base.html %} #继承父模板

{% block title%} 测试 {% end %} # 头部标题

{% block content %}
#导入自定义模块和使用方法
    {% from util.mod_file import sum %} # 导入自定义模块方法
    {{ sum(10,10)}}     # 自定义模块使用方法
{% end %}
```

mod_file.py代码
```
def sum(a,b):
    return a + b
```

### 4.6.3 函数和类导入总结
1. 渲染时导入 必须掌握:在模板渲染时导入函数和类
2. 模板中导入 必须掌握:在模板中导入内置模块和自定义的模块
```
函数和类导入：
    render(
        time = time,        # 导入一个函数模块
        hahah = self.haha,  # 导入一个函数实例
        ccc = ccc           # 导入一个函数方法
        )

        {{ haha()}}         # 函数实例的调用方法
        {{ ccc().sum(5,5)}} # 函数的使用方法

        {% import time%}    # 导入一个模块方法
        {{ time.ctime() }}  # 模块使用方法

        {% from util.mod_file import sum %} # 导入自定义模块方法
        {{ sum(10,10)}}     # 自定义模块使用方法
```

## 4.7 un_methods和ui_modules
刚才讲到了传入函数和类的两种办法，如果一个函数或类需要在很多模板中被导入那之前的两种方式会不会很繁琐呢？

### 4.7.1 ui_methods and ui_ui_modules
`ui_methods.py`这里的文件名是随意的只要在import时合法即可，这里可以新建一个文件夹，如util，来放置 ui_methods.py 和 ui_modules.py
```
# ui_methods.py定义的是函数
def func222(self):
    return 'ui_methods'
```

ui_modules.py
```
from tornado.web import UIModule # 导入UIModule模块
# ui_modules.py定义的是类
class TestMudole(UIModule):
    def render(self, *args, **kwargs):
        return 'ui_modules'
```

### 4.7.2 项目中导入和配置Application参数
test.py代码
```
from util import ui_methods,ui_modules 

class TemplatesHandler(tornado.web.RequestHandler):
    def get(self):
        self.render('subtemplate.html')

app = tornado.web.Application(
    ui_methods= ui_methods,
    ui_modules = ui_modules,
)
```

### 4.7.3 模板中函数或类调用
```
{% module TestModule() %}  # ui_modules 调用
{{ fun1() }}   # ui_methods调用
```
subtemplate.html代码
```
{% extends ./base.html %} #继承父模板

{% block title%} 测试 {% end %} # 头部标题

{% block content %}
    {{ fun1() }}

    {% module TestModule() %}
{% end %}

```

### 4.7.4 ui_methods和ui_modules总结
1. 必须掌握:`ui_methods` 的写法和使用
2. 必须掌握:`ui_modules` 的写法和作用

### 4.7.5 ui_methods和ui_modules案例
#### 4.7.5.1 添加 ui_module
在 ui_modules.py 中添加如下代码
```
class Advertisement(UIModule):
    def render(self, *args, **kwargs):
        return self.render_string('06ad.html')
    def css_files(self):
        return "css/King_Chance_Layer7.css"
    def javascript_files(self):
        return [
            "js/jquery_1_7.js",
            "js/King_Chance_Layer.js",
            "js/King_layer_test.js",
        ]
```
下载素材地址:https://github.com/Vip-ie/ad
test.py代码
```
class TemplatesHandler(tornado.web.RequestHandler):
    def get(self):
        self.render('subtemplate.html')

app = tornado.web.Application(
    handlers = [
        (r'/temp',TemplatesHandler),

    ],
    template_path = 'templates',
    static_path = 'static',
    ui_methods= ui_methods,
    ui_modules = ui_modules,
)
```
subtemplate.html代码
```
{% extends ./base.html %} #继承父模板

{% block title%} 测试 {% end %} # 头部标题

{% block content %}
    {% module Advertisement() %}
{% end %}
```

## 4.8 模板其他
### 4.8.1 apply
使用`apply`语句，使用函数的作用范围到最近的{%end%}为止

subtemplate.html代码
```
{% extends ./base.html %} #继承父模板

{% block title%} 测试 {% end %} # 头部标题

{% block content %}
   {% from util.mod_file import upper %}
   {% apply upper %}
   this is my test
   {% end %}

   {{ upper('haha')}}
{% end %}

```

mod_file.py代码
```
def upper(a):
    return a.upper()
```

### 4.8.2 linkify
linkify生成一个链接,但是要注意模板转义

subtemplate.html代码
```
{% extends ./base.html %} #继承父模板

{% block title%} 测试 {% end %} # 头部标题

{% block content %}
   {{ linkify('百度:https://www.baidu.com') }}
   {% raw linkify('百度:http://www.baidu.com') %}
{% end %}
```

### 4.8.3 模板其他命令总结
1. 了解:`apply` 的用法和作用
2. 了解:`linkify` 的用法和作用 

```
其他操作:
    {% apply upper %}
        this is my test file
    {% end %}

    linkify('http://www.baidu.com')
```

