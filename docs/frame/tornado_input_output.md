# 第二章 输入输出
Tornado可以通过write把字符串输出到浏览器，除此之外，write还可以输出到浏览器呢？

## 2 wirte
**输出总结**
>**1 write**三种类型：
>>1.字符串

>>2.bytes

>>3.字典

>>4.json字符串

>**2 flush:**
>>从缓存区刷到浏览器

>**3 finis：**
>>结束写入，后面代码依然执行，但是不会写入到浏览器

>**4 render:**
>返回一个HTML页面

>**5 rendirect：**
>路由跳转

**wirte接受对象**

write可以接受`bytes` 、`unicode`字符串和`dict`这三个对象如果接受的是字典，会把字典转化成 JSON 字符串，因此write也可以接受 JSON 字符串。

bytes例
```
class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write(b'Tornado <br>') # bytes
```

dict例
```
class MainHandler(tornado.web.RequestHandler):
    def get(self):
        di = {
            'name': 'leva',
            'age': 28
        }
        self.write(di) # 字典
```

Unicode例
```

class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write('<b>Tornado</b><br >') # Unicode
```

其他例
```
class MainHandler(tornado.web.RequestHandler):
    def get(self):
        import json
        li = [1, 2, 3]
        li = json.dumps(li)  # 把列表转成json数据
        self.write(li) 
        li=json.loads(li) # 把json数据转换成列表
```


### 2.1 flush

write 会先把内容放在缓冲区，正常情况下，当请求处理完成的时候会自动把缓冲区的内容输出到浏览器，但是可以调用 flush 方法，这样可以直接把缓冲区的内容输出到浏览器，不用等待请求处理完成
```
class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write(b'Tornado <br>')
        self.write('<b>Tornado</b><br >')
        self.flush()   # 执行flush后先把flush代码刷入浏览器在执行后面的代码
        di = {
            'name': 'leva',
            'age': 28
        }
        self.write(di)
        self.write('<br>')
        import json
        li = [1, 2, 3]
        li = json.dumps(li) 
        self.write(li)
```
Tornado执行write('***')--> 缓存区--> 浏览器


### 2.2 render

1. Tornado 能不能向浏览器输出一个已经写好的 html 文件呢？

通过`render`可以返回一个html文件
```
class TemHandler(tornado.web.RequestHandler):
    def get(self):
        self.render('in_out.html')
```

想要 Tornado 能够正确的找到 html 文件，需要在 Application 中指定文件的位置
```
Application = tornado.web.Application(
    handlers = [
        (r'/',MainHandler),
    ],
    template_path = 'templates', # 模板文件目录
)
```

### 2.3 redirect
通过`redirect`可以跳转到指定的路由
```
class MainHandler(tornado.web.RequestHandler):
    def get(self):
        import time
        time.sleep(3)  # 延迟3秒跳转
        self.redirect('/tem')

class TemHandler(tornado.web.RequestHandler):
    def get(self):
        self.write('temp')

# 路由
app = tornado.web.Application(
    handlers = [
        (r'/',MainHandler),
        (r'/tem',TemHandler),
    ],
)
```

### 2.4 finish
结束请求
```
class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write(b'Tornado <br>')
        self.write('<b>Tornado</b><br >')
        self.finish() # 执行finish后代码可以继续执行但是无法刷入到浏览器端
        print('finish')
        self.write('finish') 
```
当调用`finish`之后，请求处理完成，类似于函数中的 return (注意：请求当中不能出现return) ，其后不能再执行`write `，否则会报错

### 2.5 write输出总结
1. render:`返回HTML文件`
2. write:接受`bytes` 、`unicode`和`dict` 对象，保存到 缓冲区请求处理完成输出到浏览器
3. redirect:跳转到指定的`路由` 


## 3 获取请求信息
1. 在服务器可以知道是谁在访问吗？
2. 可以的话该如何查看呢？

**总结**
>1. request:
>>1. remote_ip(客户端的ip地址)

### 3.1 self.request
1. `method`HTTP请求方法，例如GET 或 POST 
2. `remote_ip` 客户端的IP地址，返回值类型为字符串
3. `full_url()`重新构建此请求的完整URL 
4. `request_time()`返回此请求执行所花费的时间
5. `uri`请求的完整uri
6. `path`路径部分的uri
7. `query`查询部分的uri 
8. `version`请求中指定的HTTP版本，例如“HTTP / 1.1”

self.request实例
```
class ReqHandler(tornado.web.RequestHandler):
    def get(self):
        self.write('remote_ip : ' + self.request.remote_ip)
        self.write('<br>')
        self.write('full_url : ' + str(self.request.full_url()))
        self.write('<br>')
        self.write('request_time : ' + str(self.request.request_time()))
        self.write('<br>')
        self.write('request_uri : ' + str(self.request.uri))
        self.write('<br>')
        self.write('path : ' + self.request.path)
        self.write('<br>')
        self.write('query : ' + str(self.request.query))
        self.write('<br>')
        self.write('version : ' + str(self.request.version))
        self.write('<br>')
        self.write('method : ' + self.request.method)
```

## 4 输入
输入`get_argument`可以获取 URL 中的参数，除此之外，还能获取到其他地方的数据吗?

**输入总结**
>1.get_argument
>>只返回最后一个值

>2.get_argument
>>返回一个列表
>>>get_query_argument/ get_query_arguments
>>>能获取到get请求里面的（url传参）的参数

>>get_body_argument/ get_body_arguments
>>>能获取到post请求form表单里面的参数



### 4.1.1 get_argument与get_arguments区别
1. get_argument一次只能获取一个元素，如果有多个元素获取最后一个元素。
2. get_arguments 获取一个列表元素，可以获取多个元素。
```
class GetHandler(tornado.web.RequestHandler):
    def get(self):
        name = self.get_argument('name','no')
        self.write('这是get_argument:' + name + '<br>')

        name2 = self.get_arguments('name')
        self.write('这是get_arguments:' + str(name2))
```
URL传一个参数:`http://127.0.0.1:8080/get?name=leva`

URL传多个参数:`http://127.0.0.1:8080/get?name=leva&name=rave`

### 4.1.2 get_argument通过HTML提交数据到后台
```
class GetHandler(tornado.web.RequestHandler):
    def get(self):
        self.render('in_out.html')

    def post(self):
        name = self.get_argument('name','no')
        password = self.get_argument('password','no')
        self.write('这是用户名:' + name)
        self.write('这是密码:' + password)
```

html文件代码
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Tornado</title>
</head>
<body>
 <form method="post" action="/get" >
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

### 4.1.3 URL传参
**URL传参总结**
```
?k1=v1&k2=v2&k3=v3
rest风格传参（正则表达式）
    直接写正则表达式
    ?P<key>
```
查询字符串风格
```
class GetHandler(tornado.web.RequestHandler):
    def get(self):
        name = self.get_argument('name','no')
        print(name)
```
路由
```
app = tornado.web.Application(
    handlers = [
        (r'/get',GetHandler),
    ],
)
```
RUL

`http://127.0.0.1:8000/get?name=budong`

REST风格
```
class UserHandler(tornado.web.RequestHandler):
    def get(self,name,age):
        self.write('name: %s <br> age: %s'%(name,age))

class StudentHandler(tornado.web.RequestHandler):
    def get(self,name,number):
        self.write('name: %s <br> number: %s'%(name,number))
```
路由
```
app = tornado.web.Application(
    handlers = [
        (r'/user/(.+)/([0-9]+)',UserHandler),
        (r'/stu/(?P<number>[0-9]+)/(?P<name>.+)',StudentHandler),
    ],
)
```
URL

`http://127.0.0.1:8000/user/budong/18`

`http://127.0.0.1:8000/stu/20170001/budong`

### 4.1.4 URL传参总结
1. 查询字符串: 查询字符串通过在路由后面添加  ? 在加上参数名和参数值来传入参数
2. REST:通过 / 来分割每个参数，关键在于 get 方法定义






刚才我们已经知道了如何去启动Tornado，也知道了如何通过命令行的交互来获取参数，那Tornado如何和浏览器做交互呢？

## 5 输出
从Tornado输出到浏览器我们可以使用 `write`

```
class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write('hello')
```

## 6 输入

```
class TestIndexHandler(tornado.web.RequestHandler):
    def get(self):
        name = self.get_argument('name','no')
        self.write('hello' + name)
        print(name)

        name = self.get_arguments('name')
        self.write('<br>')
        self.write(','.join(name))
        print(name)

```


使用:`http://127.0.0.1:8080/test?name=budong`

? 后面便是参数

可以通过:get_argument和get_arguments来获取参数值

## 4 输入输出总结
### 4.1 输出
1. 必须掌握:write方法

### 4.2 输入
1. 必须掌握:get_argument和get_arguments
2. 必须掌握:URL传入参数的方法