# 第一章 Tornado


## 1.0 Tornado的安装
* 启动虚拟环境命令`workon projectname`
* 安装Tornado命令`pip install tornado`

## 1.2 路由
客户端访问服务器可以看成是：客户端读取服务器资源的一个过程，路由表就指定了具体访问什么资源
```
# 路由在程序中表现形式
application = tornado.web.Application([
        (r'/',MainHanlder),
])
```
路由表是访问服务器的入口

在工作中如果有新的需求，往往只需要在路由表中添加新的路由即可

* tornado.ioloop`开启循环，让服务一直等待请求`
* tornado.web`web框架的核心模块`
```
import tornado.ioloop
import tornado.web

# 如果我们启动了一个tornado服务
# 整个执行流程都是已经被定义好的
# 通过类的方式进行一个接口的定义
# 请求与相应他又是封装到RequestHandler类里面的

class MainHandler(tornado.web.RequestHandler):
    def get(self):
    self.write('hello world')
# 以上代码指的是:在这里指定请求的资源

application = tornado.web.Application(
    [
        (r'/',MainHanlder),
    # 用来存放路径，urls
    # 路径是以元组格式存储
    ]
) #类的实例化
# 服务器进行调用的接口，web框架的应用核心

if __name__ == "__main__":
    application.listen(8080) #绑定操作
    tornado.ioloop.IOLoop.current().start() # 开启服务器

    tornado.ioloop # 不断的询问epoll 
    IOLoop.current # IOLoop是tornado.ioloop的类，current是IOLoop类的实例
    tornado.ioloop.IOLoop.current # 实例化的对象.start()
```
### 路由总结
* 必须掌握:路由的作用和定义
* 必须掌握:Handler的作用和定义

## 1.3 启动Tornado
刚才我们运行了 Tornado，在页面也看到了效果，那在实际应用中也是这么做的吗？
* 导入
```
import tornado.httpserver
import tornado.ioloop
import tornado.options
import tornado.web
```
* Handler
```
class MainHandler(tornado.web.RequestHandler):
    def get(self):
    self.write('hello world')
```
* 路由
```
app = tornado.web.Application([
        (r'/',MainHanlder),
])

if __name__ == "__main__":
    tornado.options.parse_command_line() # 打印请求信息
    http_server = tornado.httpserver.HTTPServer(app)
    http_server.listen(options.port)
    tornado.ioloop.IOLoop.current().start() 
```
### 命令行交互
使用命令行来控制启动
* Tornado支持通过命令行参数来控制
* Tornado的启动形式

代码
```
form tornado.options import define,options #导入模块前后顺序不能乱

define('port', default=8080, help='run port', type=int) 
define('version', default='0.0.1', help='version 0.0.1', type=str)
```
使用
```
python test.py --port=8000
python test.py --version=1.0
python test.py --help
```
### Tornado启动总结
* 必须掌握:Tornado的完整启动方式
* 必须掌握:Tornado 命令行参数的传入方法和方式

## 1.4 输入和输出
刚才我们已经知道了如何去启动Tornado，也知道了如何通过命令行的交互来获取参数，那Tornado如何和浏览器做交互呢？

### 输出
从Tornado输出到浏览器我们可以使用 `write`

```
class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write('hello')
```

### 输入

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

### 输入输出总结
* 输出

必须掌握:write方法

* 输入

必须掌握:get_argument和get_arguments

必须掌握:URL传入参数的方法
