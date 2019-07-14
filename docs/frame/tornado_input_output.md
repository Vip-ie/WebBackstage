# 第二章 输入输出
## 输入输出总结
### 输出
1. 必须掌握:write方法

### 输入
1. 必须掌握:get_argument和get_arguments
2. 必须掌握:URL传入参数的方法

## 1 输出`wirte`

Tornado可以通过write把字符串输出到浏览器，除此之外，write还可以输出到浏览器呢？

从Tornado输出到浏览器我们可以使用 `write`
```
class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write('hello')
```

### 1.1 输出`wirte`总结
>**1. write**三种类型：

>>1.1 字符串

>>1.2 bytes

>>1.3 字典

>>1.4 json字符串

>**2. flush:**
>>2.1 从缓存区刷到浏览器

>**3. finis：**
>>3.1 结束写入，后面代码依然执行，但是不会写入到浏览器

>**4. render:**
>>4.1 返回一个HTML页面

>**5. rendirect：**
>>5.1 路由跳转

**`wirte`接受对象**

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


### 1.2 flush

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


### 1.3 render

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

### 1.4 redirect
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

### 1.5 finish
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

### 1.6 write输出总结
1. render:`返回HTML文件`
2. write:接受`bytes` 、`unicode`和`dict` 对象，保存到 缓冲区请求处理完成输出到浏览器
3. redirect:跳转到指定的`路由` 

-------------------------------------------------------------------------------

## 2 输入`get_argument` and `get_arguments`

输入`get_argument`可以获取 URL 中的参数，除此之外，还能获取到其他地方的数据吗?
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

1. 使用URL:`http://127.0.0.1:8080/write?name=leva`
2. 获取?后面name参数值
3. 可以通过:get_argument和get_arguments来获取参数值

例:获取URL`http://127.0.0.1:8080/write?name=leva`参数name的值
```
class writeHandler(RequestHandler):
    def get(self):
        name = self.get_argument('name', 'no')
        self.write('使用get_argument获取URL参数name的值:' + name)
```

### 2.1 输入`get_argument` and `get_arguments`总结
1. `get_argument:`只返回最后一个值
2. `get_arguments:`返回一个列表
>2.1. get_query_argument/ get_query_arguments
能获取到get请求里面的（url传参）的参数

>2.2. get_body_argument/ get_body_arguments
能获取到post请求form表单里面的参数



### 2.2 get_argument and get_arguments区别
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

### 2.3 get_argument通过HTML提交数据到后台
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

### 2.4 URL传参
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
RUL:`http://127.0.0.1:8000/get?name=budong`

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
URL:`http://127.0.0.1:8000/user/budong/18`

URL:`http://127.0.0.1:8000/stu/20170001/budong`

### 2.5 URL传参总结
1. 查询字符串: 查询字符串通过在路由后面添加  ? 在加上参数名和参数值来传入参数
2. REST:通过 / 来分割每个参数，关键在于 get 方法定义

-------------------------------------------------------------------------------

## 3 请求与响应
### 3.1 请求与响应
浏览器和服务器之间是怎么沟通的？

浏览器`发起请求` --> 服务器`开始处理请求` -->服务器`处理结束`-->返回处理结果-->浏览器`展示页面`

### 3.2 请求
浏览器在发送请求的时候，会发送具体的请求信息，由请求行，请求消息头，请求正文

### 3.3 请求行
请求行,位于第一行，包含内容为：
1. `Method`:一般为GET或者POST
2. `Path-to-resource`:请求的资源的URI
3. `Http/Version-number`:客户端使用的协议的版本，HTTP/1.0和HTTP/1/1

### 3.4 请求消息头
向服务器传递附加信息
1. `Accept`:浏览器可以接受的MIME类型
2. `Acept-Charset`:浏览器支持的字符集，如gbk,utf-8 
3. `Accept-Encoding`:浏览器能够解码的数据压缩方式, 如：gzip 
4. `Accept-language`:所希望的语言
5. `Host`:请求的主机和端口
6. `User-Agent`:通知服务器，浏览器类型
7. `Content-Lenght`:表示请求消息正文的长度
8. `Connection`:表示是否需要持久连接（Keep-alive）
9. `Cookie`:这是最重要的请求信息之一（会话有关）

### 3.5 请求正文
请求具体内容，比如：URL中传入的参数，form表单里面的内容等等

### 3.6 响应信息
响应信息为服务器的处理结果。主要包含：响应行，响应消息头，响应正文

### 3.7 响应头
1. `Server`:通知客户端，服务器的类型
2. `Content-Encoding`:响应正文的压缩编码方式。常用的是gzip
3. `Content-Length`:通知客户端响应正文的数据大小 
4. `ontent-Type`:通知客户端响应正文的MIME类型 
5. `Content-Disposition`:通知客户端，以下载的方式打开资源

### 3.8 响应行
响应行主要报错如下信息：
1. `Http/Version-number`:服务器用的协议版本 
2. `message`:响应码描述。例如200的描述为OK
3. `Statuscode`:响应码。代表服务器处理的结果的一种表示，常用的响应码有： 
>* 200：正常 　　　　　　
>* 302/307：重定向 　　　　　　
>* 304:服务器的资源没有被修改 　　　　　　
>* 404：请求的资源不存在 　　　　　　
>* 500：服务器报错了 

### 3.9 响应正文
具体的响应内容，如html，JavaScript 等数据内容

### 3.10 设置响应头
1. 在请求与响应当中，请求是浏览器设置的，在服务器端只能接受，无法改变，那响应头可以改变吗？
2. 如果可以改变响应头，应该怎么操作呢？

### 3.11 set_header
set_header 可以设置响应头
```
class HeaderHandler(tornado.web.RequestHandler):
    def get(self):
        self.write('set_header')
        self.set_header('aaa','1111')
        self.set_header('bbb','222')
        self.set_header('bbb','333')
```
设置RequestHeaders时候，如果set_header设置多个相同名称和值后，只有最有一个值生效

### 3.12 add_header
add_header 可以向响应头里面添加信息，而且是可以出现相同信息
```
class HeaderHandler(tornado.web.RequestHandler):
    def get(self):
        self.write('add_header')
        self.add_header('bbb','333')
        self.add_header('bbb','333')
```
设置RequestHeaders时候，如果add_header同时设置多个相同名称和多个值，添加多个同名和多个值

### 3.13 clear_header
clear_header 可以撤销给定的响应头信息
```
class HeaderHandler(tornado.web.RequestHandler):
    def get(self):
        self.write('clear_header')
        self.add_header('abc','555')
        self.add_header('ccc','555')
        self.clear_header('abc')
```
设置RequestHeaders时候，如果clear_header设置了清除名称，在响应头内就无法看到清除的响应头

### 3.14  设置响应头总结
1. 必须掌握:set_header的作用和用法
2. 必须掌握:add_header的作用和用法
3. 必须掌握:clear_header的作用和用法

### 3.15 发送错误吗
我们可以给响应头设置信息，那我们可以不可以主动地给浏览器报错呢？
### 3.16 send_error
```
class SendHandler(tornado.web.RequestHandler):
    def get(self):
        self.write('send_error') 
        self.send_error(404) #主动抛出一个错误
```

使用 send_error 时需要注意：如果已经执行了 flush，则不能再执行 send_error，因此该方法将简单地终止响应
```
class SendHandler(tornado.web.RequestHandler):
    def get(self):
        self.write('send_error')
        self.flush()#刷新self.write('send_error')后无法在刷新self.send_error(404)到浏览器
        self.send_error(404) #主动抛出一个错误
```
如果输出已写入但尚未刷新，则将其丢弃并替换为错误页面
### 3.16 重写 write_error
1. send_error  在其底层调用的是 write_error
2. 因此只要重写此方法，就可以实现自定义的的错误页面
```
class NotFoundHandler(tornado.web.RequestHandler):
    def write_error(self, status_code, **kwargs):
        self.write('status_code:%s' % status_code)
```

### 3.17 未定义路由处理
例如在浏览器输入一个不存在的路由时候就需要主动抛出一个错我
```
class NotFoundHandler(tornado.web.RequestHandler):
    def get(self, *args, **kwargs):
        self.send_error(404)
    def write_error(self, status_code, **kwargs):
        self.render('error_notfound.html') #主动抛出一个自定义错误信息页面

app = tornado.web.Application(
    handlers = [
        (r'/(.*)',NotFoundHandler),
    ],
    template_path = 'templates',
)
```

### 3.18 状态码
![](img/status.jpg)
### 3.19 发送错误码总结
1. 必须掌握:`send_error` 的作用和用法
2. 必须掌握:`write_error` 的作用和用法
3. 了解:自定义错误页面的实现方法

### 3.20 请求处理过程
Tornado在接到请求之后，会做些什么事情呢？
### 3.21 Tornado 调用顺序
Tornado 在接受到请求之后，后按照此顺序选择响应的方法来执行
```
class IndexHandler(tornado.web.RequestHandler):
    def set_default_headers(self):
        print('---set_default_headers---: 设置header')

    def initialize(self):
        print('---initialize---: 初始化')

    def prepare(self):
        print('---prepare---: 准备工作')

    def get(self):
        self.write('---get---: 处理get请求' + '<br>')

    def post(self):
        self.write('---post---: 处理post请求' + '<br>')

    def write_error(self, status_code, **kwargs):
        print('---write_error: 处理错误')
    def on_finish(self):
        print('---on_finish---: 结束，释放资源')
```
### 3.22 请求处理过程总结
1. 大致了解接受到请求之后的方法调用顺序




















-------------------------------------------------------------------------------


## 4 获取请求信息
1. 在服务器可以知道是谁在访问吗？
2. 可以的话该如何查看呢？

### 4.1 **总结**
>1. request:
>>1. remote_ip(客户端的ip地址)

### 4.2 self.request
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
