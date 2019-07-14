# 第一章 Tornado


## 1 Tornado的安装
### 1.1 启动虚拟环境命令`workon projectname`
### 1.2 安装Tornado命令`pip install tornado`

## 2 路由
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
### 2.1 路由总结
* 必须掌握:路由的作用和定义
* 必须掌握:Handler的作用和定义

## 3 启动Tornado
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
### 3.1 命令行交互
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
### 3.2 Tornado启动总结
* 必须掌握:Tornado的完整启动方式
* 必须掌握:Tornado 命令行参数的传入方法和方式
