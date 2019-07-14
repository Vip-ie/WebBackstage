# 第二章 输入输出

刚才我们已经知道了如何去启动Tornado，也知道了如何通过命令行的交互来获取参数，那Tornado如何和浏览器做交互呢？

## 2 输出
从Tornado输出到浏览器我们可以使用 `write`

```
class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write('hello')
```

## 3 输入

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
* 输出

1. 必须掌握:write方法

* 输入

1. 必须掌握:get_argument和get_arguments
2. 必须掌握:URL传入参数的方法