# Redis
```
Redis是由意大利人Salvatore Sanfilippo（网名：antirez）
开发的一款内存高速缓存数据库。Redis全称为：Remote Dictionary Server，
该软件使用C语言编写，Redis是一个key-value存储系统，它支持丰富的数据类型，
如：string、list、set、zset(sorted set)、hash。


Redis特点:
Redis以内存作为数据存储介质，所以读写数据的效率极高，远远超过数据库。


Redis应用场景:
因为Redis交换数据快，所以在服务器中常用来存储一些需要频繁调取的数据，这样可
以大大节省系统直接读取磁盘来获得数据的I/O开销，更重要的是可以极大提升速度。
将这种热点数据存到Redis（内存）中，要用的时候，直接从内存取，极大的提高了速
度和节约了服务器的开销。
```

## MaxOSX Redis安装

**1、使用brew命令安装redis**

`brew install redis`

### 2、启动redis

后台方式启动，`brew services start redis`。

这样启动的好处是把控制台关掉后，redis仍然是启动的。当然，如果没有这样

的需求，也可以这样启动

`redis-server /usr/local/etc/redis.conf`

### 3、关闭redis

`brew services stop redis`

### 4、使用控制台连接redis

`redis-cli`

`redis-cli -h 127.0.0.1 -p`

## **Redis 五种数据类型、及操作**

### 1.string 字符串

### 2.list 列表

### 3. hash 哈希

### 4. set 集合

### 5. sorted sets 有序集合

---

## ubuntu服务器 Redis安装

```

```

安装完成后，Redis服务器会自动启动，我们检查Redis服务器程序

### 检查Redis服务器系统进程

```
redis       931  0.0  0.3  47508  3184 ?        Ssl  09:41   0:00 /usr/bin/redi-server 127.0.0.1:6379
leva       1379  0.0  0.0   6256   896 pts/0    S+   09:42   0:00 grep --color=auto redis
```

### 通过启动命令检查Redis服务器状态

```
redis-server.service - Advanced key-value store
   Loaded: loaded (/lib/systemd/system/redis-server.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2019-04-17 09:41:16 UTC; 3min 18s ago
     Docs: http://redis.io/documentation,
           man:redis-server(1)
  Process: 831 ExecStart=/usr/bin/redis-server /etc/redis/redis.conf (code=exited, status=0/SUCCESS)
 Main PID: 931 (redis-server)
    Tasks: 4 (limit: 1080)
   Memory: 3.5M
   CGroup: /system.slice/redis-server.service
           └─931 /usr/bin/redis-server 127.0.0.1:6379

Apr 17 09:41:16 leva_server systemd[1]: Starting Advanced key-value store...
Apr 17 09:41:16 leva_server systemd[1]: redis-server.service: Can't open PID file /var/run/redis/redis-serve…irectory
Apr 17 09:41:16 leva_server systemd[1]: Started Advanced key-value store.
Hint: Some lines were ellipsized, use -l to show in full.
```

### 通过命令行客户端访问Redis

* **安装Redis服务器，会自动地一起安装Redis命令行客户端程序。**
* **在本机输入redis-cli命令就可以启动，客户端程序访问Redis服务器。**

```
~ redis-cli
127.0.0.1:6379>
查看所有的key列表
redis 127.0.0.1:6379> keys *
(empty list or set)
```



