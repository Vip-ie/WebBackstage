# 第二章 输入输出
Tornado可以通过write把字符串输出到浏览器，除此之外，write还可以把那些东西输出到浏览器呢？

## 2.1 wirte
write
>三种类型：
>>1.字符串
>>2.bytes
>>3.字典
>>4.json字符串
>flush:
>>从缓存区刷到浏览器
>finis：
>>结束写入，后面代码依然执行，但是不会写入到浏览器
render:
>返回一个HTML页面
rendirect：
>路由跳转


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
### 4.1 输出
1. 必须掌握:write方法

### 4.2 输入
1. 必须掌握:get_argument和get_arguments
2. 必须掌握:URL传入参数的方法