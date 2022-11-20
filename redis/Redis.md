# 安装Redis

> 删除已经有的redis

```bash
sudo rm -f /usr/bin/redis*
```



创建目录

mkdir xxxxx

进入目录  	下载源文件压缩包

```bash
cd  /export/software  
wget  http://download.redis.io/releases/redis-6.2.6.tar.gz 
```

解压

```bash
cd /export/software  
tar xzf redis-6.2.6.tar.gz
```

安装C运行环境

```bash
apt install gcc-c++  
```

安装tcl

```bash
apt install tcl
```

其它环境依据 提示安装



进入源码文件夹

编译源码并安装 包括具体安装位置

```bash
cd /export/server/redis-6.2.6/  
make test && make install PREFIX=/export/server/redis-6.2.6  
```

修改配置文件

```bash
vim  redis.conf  
#  修改第61行  
bind  localhost  
#  修改第128行  后台 
daemonize  yes  
#  修改第163行  
logfile  "/export/server/redis-6.2.6/log/redis.log"  
#  修改第247行  
dir  /export/server/redis-6.2.6/data  
```



复制、移动文件

````bash
cp(移动mv) /xx/文件夹路径/文件名 [新路径](当前路径 ./ )

cp(移动mv) /xx/文件夹路径/*(该文件夹下的所有文件) [新路径](当前路径 ./ )
````







```yml
spring:
  redis:
    host: 192.168.245.131
    port: 6379
    timeout: 1000ms
    
    jedis:
      pool:
        max-active: 8
        max-idle: 8
        max-wait: -1ms
        min-idle: 0

```



启动

```bash
cd /export/server/redis-6.2.6/  
./bin/redis-server ./bin/redis-server/redis.conf  
```

关闭

```bash
./bin/redis-cli  -h localhost shutdown  
```



# 删除文件

1. 删除文件夹的内容包括文件夹：

rm -rf 文件夹的名字    （-r 是 循环的意思， f是不询问的意思）

2 .删除文件夹的内容不包括文件夹：

rm -rf 文件夹的名字/*   (后面加上/*表示删除内容不删除文件夹)



# 修改系统时区

查看系统时区

````bash
date -R
````

设置时区

````bash
tzselect
````









# 关闭redis保护模式

redis在启动的时候默认会启动一个保护模式，只有同一个服务器可以连接上redis。别的服务器连接不上这个redis
解决办法：关闭保护模式

1、进入redis安装目录找到conf目录下的redis.conf（/etc/redis/redis.conf）文件
     vi redis.conf 注释bind 127.0.0.1这一行

2、启动redis 登入客户端  执行下面命令
    config set protected-mode "no" 

# Redis

>redis是一种数据库。采用K-V模型存储数据

redis是一种数据库。采用K-V模型存储数据

Remote Dictionary Server远程字典服务器，高性能的NoSQL数据库，**基于内存**

Redis中的数据大部分时间都是存储在内存中。适合存储频繁访问、数据量比较小的数据。



# NoSQL概述

> 1、单机MySql

![image-20220507142737432](C:\Users\qjj\Desktop\学习\Redis.assets\image-20220507142737432.png)

>2、Memcached(缓存) +MySQL + 垂直拆分(读写分离)

网站80%的情况都是在读，减轻数据库的压力，使用缓存来保证效率。

发展过程：优化数据结构和索引------>文件缓存(IO)---->Memcached

![image-20220507140646116](C:\Users\qjj\Desktop\学习\Redis.assets\image-20220507140646116.png)



>3、分库分表+水平拆分+MySQL集群

技术和业务下发展的同时，对人的要求也越来越高！

本质：数据库(读、写)

早些年MyISAM：表锁，十分影响效率！高并发下就会出现严重的锁问题

转战Innodb:行锁



![image-20220507142639140](C:\Users\qjj\Desktop\学习\Redis.assets\image-20220507142639140.png)



>4、如今

MySQL等关系型数据库就不够用了！数据量很多，变化很快！

MySQL有的使用存储大量的数据 （博客、图片） 数据库表很大，效率就低了



> 目前一个基本的互联网项目！



![image-20220507144120927](C:\Users\qjj\Desktop\学习\Redis.assets\image-20220507144120927.png)

>为什么要用NoSQL！

用户的个人信息，社交网络，地理位置。用户自己产生的数据，用户日志信息的爆发增长



# 什么是NoSQL

> NoSQL

NoSQL = Not Only SQL

关系型数据库：表格，行，列

泛指非关系型数据库，随着web2.0互联网的诞生，尤其是超大规模的高并发的社区。

NoSQL在当今大数据环境下发展迅速，Redis是发展最快的，而且是我们当下必须要掌握的一个技术！

很多的数据类型 用户的个人信息，社交网络，地理位置。这些数据类型的存储不需要一个固定的格式！不需要多余的操作就可以横向扩展`Map<String,Object>`使用键值队来控制 



> NoSQL特点

解耦

1、方便扩展（数据之间没有关系，很好扩展）

2、大数量高性能(Redis 一秒可以写8万次，读11万次 ，NoSQL的缓存记录级，是一种细粒度的缓存，性能会比较高。)

3、数据类型是多样的！(不需要事先设计数据库！随取随用！)

4、传动RDBMS和NoSQL

```
传统的RDBMS
- 结构化组织
- SQL
- 数据和关系都存在单独的表中 row col
- 操作，数据定义语言
- 严格的一致性
- ......
```



```
NoSQL
- 不仅仅是数据库
- 没有固定的查询语句
- 键值对存储，列存储，文档存储，图形数据库
- 最终一致性
- CAP定理和 BASE（异地多活）
- 高性能，高可用、高可扩展性
- ..........
```



>了解：3V+3高

大数据时代的3V ：主要描述问题

1. 海量（Volume）
2. 多样（Variety）
3. 实时（Velocity）

大数据时代的3高 ：主要对程序的要求

1. 高并发
2. 高可扩
3. 高性能



真正的时间：NoSQL+RDBMS 一起使用才是最强的



# NoSQL的四大分类

### KV键值对：

新浪：Redis

美团：Redis+Tair

阿里、百度：Redis+memecache

### 文档型数据库(bson格式和Json一样)

- MongoDB
  - MongoDB是一个基于分布式文件存储的数据库，主要用来处理大量的文档！
  - MongoDB是一个介于关系型和非关系型数据库之间的中间产品！MongoDB是非关系型数据库中功能最丰富，最像关系型数据库的。

- ConthDB

### 列表存储数据库

- HBase
- 分布式文件系统

### 图关系数据库

![image-20220507161703840](C:\Users\qjj\Desktop\学习\Redis.assets\image-20220507161703840.png)

- 不是存图形，存放的是关系，比如：朋友圈社交网络
- Neo4j，InfoGrid



> 四者对比

![image-20220507161922800](C:\Users\qjj\Desktop\学习\Redis.assets\image-20220507161922800.png)



# Redis入门

## 概述

>redis是一种数据库。采用K-V模型存储数据

redis是一种数据库。采用K-V模型存储数据

Remote Dictionary Server远程字典服务器，高性能的NoSQL数据库，**基于内存**

Redis中的数据大部分时间都是存储在内存中。适合存储频繁访问、数据量比较小的数据。

> Redis能做什么？

1、内存存储、持久化 (rdb、aof)

2、效率高，可用于高速缓存

3、发布订阅系统

4、地图信息分析

5、计时器、计数器

6、........

> 特性

1、多样的数据类型

2、持久化

3、集群

4、事务

..........

> 网站

中文官网：https://www.redis.net.cn/

官网：https://redis.io/

安装：

```bash
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list

sudo apt-get update
sudo apt-get install redis
```





```bash
启动： redis-server
		redies-cli -p 端口号
关闭   shutdown
```

性能测试

![image-20220508004955425](C:\Users\qjj\Desktop\学习\Redis.assets\image-20220508004955425.png)

```bash
测试：100个
redis-benchmark -h localhost -p 6379 -c 100 -n 100000
```

```
ps -ef当前系统所有进程
ps -ef | grep xxxx 查看具体进程
kill -9 32 杀死32号进程（具体更多参数kill -l）
pkill -9 -u kaen 杀死kaen用户的所有进程，被强制退出
killall -a -9 sendmail 杀死sendmail全部进程
jobs 显示后台任务
fg 靠前任务切换到前台
fg n 第n号任务切换到前台
Ctrl +z 暂停正在执行任务并切换到后台
bg 后台执行最靠前的任务
bg n
nohup oder1&oder2…
```

# 五大数据类型



## Redis-key

```bash
127.0.0.1:6379> set name test  #设置键值
OK
127.0.0.1:6379> keys *  #查看全部键
1) "key:__rand_int__"
2) "mylist"
3) "key"
4) "name"
5) "counter:__rand_int__"
6) "myhash"
127.0.0.1:6379> EXISTS name  #判断是否存在
(integer) 1
127.0.0.1:6379> EXPIRE name 10  #设置过期时间
(integer) 1
127.0.0.1:6379> ttl name
(integer) 7
127.0.0.1:6379> ttl name
(integer) 6
127.0.0.1:6379> ttl name
(integer) 5
127.0.0.1:6379> ttl name
(integer) 3
127.0.0.1:6379> ttl name
(integer) -2
127.0.0.1:6379> get neame
(nil)
127.0.0.1:6379> 
127.0.0.1:6379> type name #键类型
string

```



## String

```bash
127.0.0.1:6379> APPEND name "hello"  #追加字符串，不存在，则增加
(integer) 9
127.0.0.1:6379> get name
"testhello"
127.0.0.1:6379> strlen name #获取字符串长度
(integer) 9
127.0.0.1:6379> 
##########################################################

127.0.0.1:6379> set views 0 #初始浏览量 0
OK
127.0.0.1:6379> get views
"0"
127.0.0.1:6379> incr views  #自加1
(integer) 1
127.0.0.1:6379> incr views
(integer) 2
127.0.0.1:6379> get views
"2"
127.0.0.1:6379> decr views #自减1
(integer) 1
127.0.0.1:6379> incrby views 10 #设置步长，指定增加量
(integer) 11
127.0.0.1:6379> decrby views 5
(integer) 6
127.0.0.1:6379> 
#########################################################
#字符串范围  GETRANGE
127.0.0.1:6379> set key1 "helloworld"
OK
127.0.0.1:6379> GETRANGE key1 0 3   #[0,3]
"hell"
127.0.0.1:6379> GETRANGE key1 0 -1  #全部
"helloworld"
127.0.0.1:6379> 
#######################################################################
# 替换
127.0.0.1:6379> SETRANGE key1 1 xxx #从指定位置开始替换
(integer) 10
127.0.0.1:6379> get key1
"hxxxoworld"
127.0.0.1:6379> 
#########################################################
#setex （set with expire)  #设置过期时间
#setnc	(set if not exist) #不存在再设置(在分布式锁中会常常使用)

127.0.0.1:6379> setex key3 30 "hello" #设置key3 值为hello 30 秒过期
OK
127.0.0.1:6379> ttl key3
(integer) 24
127.0.0.1:6379> get key3
"hello"
127.0.0.1:6379> setnx mykey4 "redis" #如果mukey4不存在，创建成功
(integer) 1
127.0.0.1:6379> get mykey4
"redis"
127.0.0.1:6379> setnx mykey4 "newredis" #如果mukey4存在，创建失败
(integer) 0
127.0.0.1:6379> get mykey4
"redis"
127.0.0.1:6379> 
#########################################
mset
mget
127.0.0.1:6379> mset k1 v1 k2 v2 k3 v3 #同时设置多个值
OK
127.0.0.1:6379> mget k1 k2 k3 #同时获取多个值
1) "v1"
2) "v2"
3) "v3"
127.0.0.1:6379> msetnx k1 v1 k4 v4  #msetnx 是一个原子性操纵
(integer) 0
127.0.0.1:6379> 

#对象
set user:1{name:zhangsan,age:3} #设置一个user:1 对象，值为JSON字符保存一个对象

#这里的 key 是一个巧妙的设计 : user:{id}:{filed}
127.0.0.1:6379> mset user:1:name zhangsan user:1:age 21
OK
127.0.0.1:6379> mget user:1:name user:1:age
1) "zhangsan"
2) "21"
127.0.0.1:6379> 

###############################################################################
getset #先get再set
127.0.0.1:6379> getset db redis #如果不存在，则返回nil
(nil)
127.0.0.1:6379> get db
"redis"
127.0.0.1:6379> getset db mongodb #如果存在值，获取原来的值，再修改新的值
"redis"
127.0.0.1:6379> get db
"mongodb"


```











## List

> 在 Redis 里面，可以把 List 当成**栈**、**队列**、**阻塞队列**使用。
>
> list 实际是一个链表，左右都可以插入值。
>
> 如果 key 不存在，创建新的链表。
>
> 如果移除了所有元素，空链表也代表不存在。
>
> 在两边插入或者改动值，效率最高；操作中间元素，效率相对低一些。
>
> 应用场景：消息排队

### 从左插入

>  Lpush

将一个值或者多个值，插入列表的头部，即从左插入。

```bash
127.0.0.1:6379> LPUSH list one # 从左插入一个值
(integer) 1
127.0.0.1:6379> LPUSH list two three # 从左插入多个值
(integer) 3
127.0.0.1:6379> LRANGE list 0 -1 # -1 即表示查询所有元素
1) "three"
2) "two"
3) "one"
127.0.0.1:6379> 
127.0.0.1:6379> LRANGE list 0 1 # 查询指定下标范围元素
1) "three"
2) "two"
127.0.0.1:6379> 

```

先进的排在后面，后进的排在前面。

### 从右插入

> Rpush

将一个值或者多个值，插入列表的尾部，即从右插入。

```bash
127.0.0.1:6379> RPUSH list four # 从右插入一个值
(integer) 4
127.0.0.1:6379> RPUSH list five six # 从右插入多个值
(integer) 6
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "two"
3) "one"
4) "four"
5) "five"
6) "six"
127.0.0.1:6379> 
```

先进的排在前面，后进的排在后面。

### 元素前后插入值

> Linsert

```bash
127.0.0.1:6379> LRANGE list1 0 -1
1) "two"
2) "one"
127.0.0.1:6379> LINSERT list1 before two three  # two 之前插入 three
(integer) 3
127.0.0.1:6379> LRANGE list1 0 -1
1) "three"
2) "two"
3) "one"
127.0.0.1:6379> LINSERT list1 after two three  # two 之后插入 three
(integer) 4
127.0.0.1:6379> LRANGE list1 0 -1
1) "three"
2) "two"
3) "three"
4) "one"
127.0.0.1:6379> 

```

### 指定下标赋值

> Lset

```bash
127.0.0.1:6379> Lrange list 0 -1
1) "two"
127.0.0.1:6379> Lset list 0 one # 赋值列表指定下标元素
OK
127.0.0.1:6379> Lrange list 0 -1
1) "one"
```

如果列表不存在或者列表指定下标不存在，赋值失败。

### 取值

### 查看列表

> Lrange

```bash
127.0.0.1:6379> Lrange list 0 -1 # -1 即表示查询所有元素
1) "three"
2) "two"
3) "one"
127.0.0.1:6379> Lrange list 0 1 # 查询指定下标范围元素
1) "three"
2) "two"
```

### 下标获取元素

> Lindex

```bash
127.0.0.1:6379> Lrange list 0 -1
1) "two"
2) "one"
127.0.0.1:6379> Lindex list 0 # 下标从 0 开始
"two"
127.0.0.1:6379> Lindex list 1
"one"
```

Redis 显示的下标是从 1 开始的，实际的下标还是从 0 开始的。

### 列表长度

> Llen

```bash
127.0.0.1:6379> Llen list
(integer) 2
```

### 列表是否存在

> exists

```bash
127.0.0.1:6379> exists list
(integer) 1
127.0.0.1:6379> exists list3
(integer) 0
```

### 删除

### 从左移除

> Lpop

```bash
127.0.0.1:6379> Lrange list 0 -1
1) "three"
2) "two"
3) "one"
4) "four"
127.0.0.1:6379> Lpop list # 移除最左边的元素
"three"
127.0.0.1:6379> Lrange list 0 -1
1) "two"
2) "one"
3) "four"
```

### 从右移除

> Rpop

```bash
127.0.0.1:6379> Lrange list 0 -1
1) "two"
2) "one"
3) "four"
127.0.0.1:6379> Rpop list # 移除最右边的元素
"four"
127.0.0.1:6379> Lrange list 0 -1
1) "two"
2) "one"
```

### 移除元素

> Lrem

```bash
127.0.0.1:6379> Lrange list 0 -1
1) "three"
2) "three"
3) "two"
4) "one"
127.0.0.1:6379> Lrem list 1 one # 移除一个指定元素
(integer) 1
127.0.0.1:6379> Lrange list 0 -1
1) "three"
2) "three"
3) "two"
127.0.0.1:6379> Lrem list 2 three # 移除两个指定元素
(integer) 2
127.0.0.1:6379> Lrange list 0 -1
1) "two"
```

### 截取

### 截取下标范围的元素

> Ltrim

```bash
127.0.0.1:6379> Lrange list 0 -1
1) "one"
2) "two"
3) "three"
4) "four"
127.0.0.1:6379> Ltrim list 1 2 # 截取下标 1 到 2 的元素
OK
127.0.0.1:6379> Lrange list 0 -1
1) "two"
2) "three"
```

### 移动

### 移除列表最后一个元素并移动到新列表中

> Rpoplpush

```bash
127.0.0.1:6379> Lrange list 0 -1
1) "two"
2) "three"
127.0.0.1:6379> Rpoplpush list list2 # 移除列表最后一个元素并移动到新列表中
"three"
127.0.0.1:6379> Lrange list 0 -1 # 原来的列表
1) "two"
127.0.0.1:6379> Lrange list2 0 -1 # 新的列表
1) "three"
```

## Set

> Set 中的值是不能重复的
>
> 应用场景：共同关注

### 赋值

### 插入值

> Sadd (key1 value1)

```bash
127.0.0.1:6379> Sadd set hello
(integer) 1
127.0.0.1:6379> Sadd set world
(integer) 
1
127.0.0.1:6379> Sadd set world # 插入了重复值，没有生效
(integer) 0
127.0.0.1:6379> Smembers set
1) "world"
2) "hello"
```

### 取值

### 所有元素

> Smembers

```bash
127.0.0.1:6379> Smembers set
1) "world"
2) "hello"
```

### 元素是否存在

> Sismember

```bash
127.0.0.1:6379> Smembers set
1) "world"
2) "hello"
127.0.0.1:6379> Sismember set hello # 存在返回 1
(integer) 1
127.0.0.1:6379> Sismember set hello1 # 不存在返回 0
(integer) 0
```

### 元素个数

> Scard

```bash
127.0.0.1:6379> Scard set
(integer) 2
```

### 取随机元素 

> Srandmember

```
127.0.0.1:6379> Smembers set
1) "world"
2) "hello"
127.0.0.1:6379> Srandmember set
"world"
127.0.0.1:6379> Srandmember set
"hello"
```

### 两个集合的差集

> Sdiff

```bash
127.0.0.1:6379> Smembers set1
1) "b"
2) "a"
127.0.0.1:6379> Smembers set2
1) "b"
2) "c"
127.0.0.1:6379> Sdiff set1 set2 # 取 set1 对于 set2 的差集
1) "a"
127.0.0.1:6379> Sdiff set2 set1 # 取 set2 对于 set1 的差集
1) "c"
```

### 两个集合的交集

> Sinter

```bash
127.0.0.1:6379> Smembers set1
1) "b"
2) "a"
127.0.0.1:6379> Smembers set2
1) "b"
2) "c"
127.0.0.1:6379> Sinter set1 set2 # 取 set1 和 set2 的交集
1) "b"
```

可以用来获取**共同关注**。

### 两个集合的并集

> Sunion

```
127.0.0.1:6379> Smembers set1
1) "b"
2) "a"
127.0.0.1:6379> Smembers set2
1) "b"
2) "c"
127.0.0.1:6379> Sunion set1 set2
1) "b"
2) "c"
3) "a"
```

### 删除

### 指定元素

> Srem

```bash
127.0.0.1:6379> Srem set world
(integer) 1
127.0.0.1:6379> Smembers set1) 
"hello"
```

### 随机元素

> Spop

```bash
127.0.0.1:6379> Smembers set
1) "world"
2) "hello"
127.0.0.1:6379> Spop set
"world"
```

### 移动

### 指定元素到其他集合

> Smove

```bash
127.0.0.1:6379> Smembers set
1) "hello"
127.0.0.1:6379> Smove set set1 hello # 移动 set 中的 hello 到 set1 中（set1 是存在的）
(integer) 1
127.0.0.1:6379> Smembers set1
1) "hello"
2) "world"
127.0.0.1:6379> Smembers set(empty array)
127.0.0.1:6379> Smove set1 set2 hello # 移动 set1 中的 hello 到 set2 中（set2 不存在则创建）
(integer) 1
127.0.0.1:6379> Smembers set21) 
"hello"
```

## Hash

> 哈希就是 key - map 的数据结构
>
> 应用场景：对象存储

### 赋值

### 单个哈希

> Hset

```bash
127.0.0.1:6379> Hset hash f1 sail
(integer) 1
```

### 多个哈希

> Hmset

```bash
127.0.0.1:6379> HSET hash f2 sail2 f3 sail3
(integer) 2
```

### 不存在才赋值

> Hsetnx

```bash
127.0.0.1:6379> Hkeys hash
1) "f1"
2) "f2"
3) "f3"
127.0.0.1:6379> Hsetnx hash f4 1 # f4 不存在，赋值成功
(integer) 1
127.0.0.1:6379> Hget hash f4
"1"
127.0.0.1:6379> Hsetnx hash f4 2 # f4 存在，赋值失败
(integer) 0
127.0.0.1:6379> Hget hash f4
"1"
```

### 自增

> Hincrby

```bash
127.0.0.1:6379> Hset hash f3 1
(integer) 1
127.0.0.1:6379> Hincrby hash f3 1 # 自增 1
(integer) 2
127.0.0.1:6379> Hincrby hash f3 -1 # 自减 1（哈希没有自减命令，用自增负数实现自减）
(integer) 1
```

### 取值

### 单个哈希

> Hget

```bash
127.0.0.1:6379> Hget hash f1
"sail"
```

### 多个哈希

> Hmget

```bash
127.0.0.1:6379> Hmget hash f2 f3
1) "sail2"
2) "sail3"
```

### 所有值

> Hgetall

```bash
127.0.0.1:6379> Hgetall hash
1) "f1"
2) "sail"
3) "f2"
4) "sail2"
5) "f3"
6) "sail3"
```

### 所有 key

> Hkeys

```bash
127.0.0.1:6379> Hkeys hash
1) "f1"
2) "f2"
```

### 所有 value

> Hvals

```bash
127.0.0.1:6379> Hvals hash
1) "sail"
2) "sail2"
```

### 长度

> Hlen

```bash
127.0.0.1:6379> Hgetall hash
1) "f1"
2) "sail"
3) "f2"
4) "sail2"
127.0.0.1:6379> Hlen hash
(integer) 2
```

### 字段是否存在

> Hexists

```bash
127.0.0.1:6379> Hgetall hash
1) "f1"
2) "sail"
3) "f2"
4) "sail2"
127.0.0.1:6379> Hexists hash f1 # 存在返回 1
(integer) 1
127.0.0.1:6379> Hexists hash f3 # 不存在返回 0
(integer) 0
```

### 删除

### 指定字段

> Hdel

```bash
127.0.0.1:6379> Hdel hash f3
(integer) 1
127.0.0.1:6379> Hgetall hash
1) "f1"
2) "sail"
3) "f2"
4) "sail2"
```













## Zset

> Zset 就是 Set 的有序集合
>
> 应用场景：排行榜

### 赋值

### 一个或多个元素

> Zadd   ( key1 score1 value1)

```bash
127.0.0.1:6379> Zadd zset 1 one
(integer) 1
127.0.0.1:6379> Zadd zset 2 two 3 three
(integer) 2
```

### 取值

### 正序查询指定下标范围

> Zrange

```bash
127.0.0.1:6379> Zrange zset 0 1
1) "one"
2) "two"
127.0.0.1:6379> Zrange zset 0 -1 # 0 -1 即为查询所有的元素
1) "one"
2) "two"
3) "three"
```

### 倒序查询指定下标范围

> Zrevrange

```bash
127.0.0.1:6379> Zrevrange zset 0 -1
1) "three"
2) "two"
3) "one"
```

### 元素个数

> Zcard

```bash
127.0.0.1:6379> Zrange zset 0 -1
1) "one"
2) "two"
127.0.0.1:6379> Zcard zset
(integer) 2
```

### 指定区间元素数量

> Zcount

```bash
127.0.0.1:6379> Zrange zset 0 -1
1) "one"
2) "two"
3) "three"
127.0.0.1:6379> Zcount zset 1 3 # 这里下标从 1 开始
(integer) 3
127.0.0.1:6379> Zcount zset 1 2
(integer) 2
```

### 排序

### 正序

> Zrangebyscore

```bash
127.0.0.1:6379> Zrangebyscore zset -inf +inf # 负无穷到正无穷正序排列
1) "one"
2) "two"
3) "three"
127.0.0.1:6379> Zrangebyscore zset -inf 2 # 负无穷到 2 排序
1) "one"
2) "two"
127.0.0.1:6379> Zrangebyscore zset -inf 2 limit 0 1 # 排序结果只取 0 到 1 条
1) "one"
127.0.0.1:6379> Zrangebyscore zset -inf +inf withscores # 排序结果带上分数
1) "one"
2) "1"
3) "two"
4) "2"
5) "three"
6) "3"
```

### 倒序

> Zrevrangebyscore

```bash
127.0.0.1:6379> Zrevrangebyscore zset +inf -inf # 正无穷到负无穷倒序排列
1) "three"
2) "two"
3) "one"
```

### 删除

### 指定元素

> Zrem

```bash
127.0.0.1:6379> Zrange zset 0 -1
1) "one"
2) "two"
3) "three"
127.0.0.1:6379> Zrem zset three # 移除指定元素
(integer) 1
127.0.0.1:6379> Zrange zset 0 -1
1) "one"
2) "two"
```



RedisTemplate 使用的  lettuce 客户端库

```xml
<!--redis起步依赖： 直接在项目中使用RedisTemplate(StringRedisTemplate)-->
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
data-redis使用的   lettuce 客户端库

在程序中使用RedisTemplate类的方法 操作redis数据， 实际就是调用的lettuce 客户端的中的方法
```

springboot 集成 Redis

```yaml
#指定redis （host ，ip， password）

spring:
  redis:
    host: 192.168.245.131
    port: 6379
#spring.redis.password=123
```







![image-20220810173501712](Redis.assets/image-20220810173501712.png)





# 常用





## 常用String

````bash
1 设置值 获取值
set key value
get key

2 mset mget 一次性操作多组
mset key1 value1 key2 value2
mget key1 key2

3 没有这个键我们才设置
setnx key value

4 key的长度
strlen key

5 将key的值加一操作  减一操作  #业务举例  商城商品的售出 如下图
incr stock
incrby stock 10 #设置步长，指定增加量

decr stock
decrby stock 10 #设置步长，指定增加量

6 设置key的值存货时间5秒，值为value   #业务举例  手机验证码  超时设置
setex key second value
````

![image-20220810180345540](Redis.assets/image-20220810180345540.png)





## 常用Hash

![image-20220810181147154](Redis.assets/image-20220810181147154.png)



````bash
1、设置值 获取值
	hset key(键) field(字段、小key) value(值)

hset user username zhangsan
hset user age 18
hget user username

2、批量
hmset user1 username zhangsan age 19 

3、获取所有的键值对
hgetall user
192.168.245.131:6379> hgetall user
1) "username"
2) "zhangsan"
3) "age"
4) "18"



4、获取所有小key
hkeys  user
192.168.245.131:6379> hkeys user
1) "username"
2) "age"



5、获取所有值
HVALS user
192.168.245.131:6379> hvals user
1) "zhangsan"
2) "18"



6、删除 
hdel user age

````



## 常用List

Redis列表是简单的字符串列表，按照插入**顺序**排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）

````bash
1 设置值
lpush list1 1 2 3 4 1
rpush list1 6

2查看数据
lrange list1 0 -1  取值为[a,b]

3 移除数据
lpop list1
rpop list1
````

![image-20220810200320407](Redis.assets/image-20220810200320407.png)



## 常用Set

- Redis 的 Set 是 String 类型的**无序**集合。集合成员是**唯一**的，这就意味着集合中不能出现重复的数据

- Redis 中集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。

  

````bash
1添加数据
sadd set1 1 2 3 4 5

2获取数据
smembers set1

3获取成员数量
scard set1

4业务 uv 当天登陆用户数
sadd uv:20220222 001 002 003 002
scard uv:20220222
````



## 常用Key的操作

````bash
1删除
del key

2查看所有的key  
keys *     生产环境下，别用

3存在key
exists key

4存活时间 为给定 key 设置过期时间，以秒计。
expire key seconds

5剩余存活时间   以毫秒为单位返回 key 的剩余的过期时间。 登陆续期
pttl key
ttl key 	以秒为单位返回 key 的剩余的过期时间

6随机获取 key  从当前数据库中随机返回一个 key
randomkey
````



## 对ZSet的操作-重要（热搜）

- Redis有序集合和集合一样也是string类型元素的集合,且不允许重复的成员
- 它用来保存需要排序的数据，例如排行榜，一个班的语文成绩，一个公司的员工工资，一个论坛的帖子等。
- 有序集合中，每个元素都带有score（权重），以此来对元素进行排序
- 它有三个元素：key、member和score。以语文成绩为例，key是考试名称（期中考试、期末考试等），member是学生名字，score是成绩。
  - 互联网，微博热搜，最热新闻，统计网站pv(pageview 网页)



```bash
1添加
zadd key score(权重) member
zadd pv 100 page1.html 200 page2.html 300 page3.html

2获取有序集合的成员数
zcard pv

3查询指定权重范围的成员数    取值[min max]
ZCOUNT key min max 计算在有序集合中指定区间分数的成员数
ZCOUNT pv 150 500

4增加权重 为zset中成员增加权重
ZINCRBY pv 1 page1.html

5交集
ZINTERSTORE destination 'numkeys' key [key ...] 计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 key 中
ZADD pv_zset1 10 page1.html 20  page2.html
ZADD pv_zset2 5 page1.html 10  page2.html
ZINTERSTORE pv_zset_result 2 pv_zset1  pv_zset2



6成员的分数值
ZSCORE pv_zset page3.html   

7 获取下标范围内的成员。 排序，默认权重由低到高
ZRANGE pv 0 -1

8'反转  获取'获取由高到低的几个成员（reverse）使用最多的
效率很高，因为本身zset就是排好序的。
ZREVRANGE key start stop
```





## 对位图BitMaps的操作

- 计算机最小的存储单位是位bit，Bitmaps是针对位的操作的，相较于String、Hash、Set等存储方式更加节省空间

- Bitmaps不是一种数据结构，操作是基于String结构的，一个String最大可以存储512M，那么一个Bitmaps则可以设置2^32个位

- Bitmaps单独提供了一套命令，所以在Redis中使用Bitmaps和使用字符串的方法不太相同。可以**把Bitmaps想象成一个以位为单位的数组**，数组的每个单元**只能存储0和1**，数组的下标在Bitmaps中叫做偏移量offset

  ![image-20220222170630408](Redis.assets/image-20220222170630408.855a0075.png)

- BitMaps 命令说明：**将每个独立用户是否访问过网站存放在Bitmaps中， 将访问的用户记做1， 没有访问的用户记做0， 用偏移量作为用户的id** 。

  unique:users:2022-04-05 0 1 0 0

#### [#](https://www.ydlclass.com/doc21xnv/frame/redis/#_7-1-设置值)7.1 设置值

```text
SETBIT key offset(位移) value  
```

**setbit**命令设置的vlaue只能是**0**或**1**两个值

- 设置键的第offset个位的值（从0算起），假设现在有20个用户，**uid=0，5，11，15，19**的用户对网站进行了访问， 那么当前Bitmaps初始化结果如图所示

  ![image-20220222171326788](Redis.assets/image-20220222171326788.424b1aed.png)

- 具体操作过程如下， **unique:users:2022-04-05**代表2022-04-05这天的独立访问用户的Bitmaps

  ```text
   setbit unique:users:2022-04-05 0  1  
   setbit unique:users:2022-04-05 5 1  
   setbit unique:users:2022-04-05 11 1  
   setbit unique:users:2022-04-05 15 1  
   setbit unique:users:2022-04-05 19 1 
  ```

- 很多应用的用户id以一个指定数字（例如10000） 开头， 直接将用户id和Bitmaps的偏移量对应势必会造成一定的浪费， 通常的做法是每次做setbit操作时将用户id减去这个指定数字。

  10000000 10000005 10000011

- 在第一次初始化Bitmaps时， 假如偏移量非常大， 那么整个初始化过程执行会比较慢， 可能会造成Redis的阻塞。

#### [#](https://www.ydlclass.com/doc21xnv/frame/redis/#_7-2-获取值)7.2 获取值

```text
GETBIT key offset
```

获取键的第offset位的值（从0开始算），例：下面操作获取id=8的用户是否在2022-04-05这天访问过， 返回0说明没有访问过。

```text
getbit unique:users:2022-04-05 8
```

![image-20220222171409985](Redis.assets/image-20220222171409985.ce0a3e4b.png)

#### [#](https://www.ydlclass.com/doc21xnv/frame/redis/#_7-3-获取bitmaps指定范围值为1的个数)7.3 获取Bitmaps指定范围值为1的个数

```bash
BITCOUNT key [start end] 
```

例：下面操作计算2022-04-05这天的独立访问用户数量：

```text
bitcount unique:users:2022-04-05  
```



![image-20220222171507374](Redis.assets/image-20220222171507374.103ab048.png)

#### [#](https://www.ydlclass.com/doc21xnv/frame/redis/#_7-4-bitmaps间的运算)7.4 Bitmaps间的运算

```bash
BITOP operation(操作名) destkey(新键) key [key, …] 


```



bitop是一个**复合操作**， 它可以做多个Bitmaps的and（交集） 、 or（并集） 、 not（非） 、 xor（异或） 操作并将结果保存在destkey中。

需求：假设2022-04-04访问网站的userid=1， 2， 5， 9， 如图3-13所示：

```bash
setbit unique:users:2022-04-04 1 1  
setbit  unique:users:2022-04-04 2 1  
setbit  unique:users:2022-04-04 5 1  
setbit  unique:users:2022-04-04 9 1  
```

例1：下面操作计算出2022-04-04和2022-04-05两天都访问过网站的用户数量， 如下所示。

```bash
bitop and unique:users:and:2022-04-04_05  unique:users:2022-04-04 unique:users:2022-04-05  
bitcount unique:users:and:2022-04-04_05
```

![image-20220222172009994](Redis.assets/image-20220222172009994.8449f625.png)

例2：如果想算出2022-04-04和2022-04-05任意一天都访问过网站的用户数量（例如月活跃就是类似这种） ， 可以使用or求并集， 具体命令如下：

```text
 bitop or unique:users:or:2022-04-04_05  unique:users:2022-04-04 unique:users:2022-04-05  
 bitcount unique:users:or:2022-04-04_05
```



![image-20220222172134891](Redis.assets/image-20220222172134891.231c6e16.png)

## [#](https://www.ydlclass.com/doc21xnv/frame/redis/#_8、对hyperloglog结构的操作)8、对HyperLogLog结构的操作

#### [#](https://www.ydlclass.com/doc21xnv/frame/redis/#_8-1-应用场景)8.1 应用场景

HyperLogLog常用于大数据量的统计，比如页面访问量统计或者用户访问量统计。

要统计一个页面的访问量（PV），可以直接用redis计数器或者直接存数据库都可以实现，如果要统计一个页面的用户访问量（UV），一个用户一天内如果访问多次的话，也只能算一次，这样，我们可以使用SET集合来做，因为SET集合是**有去重功能**的，**key存储页面对应的关键字，value存储对应的userid，这种方法是可行的**。但如果访问量较多，假如有几千万的访问量，这就麻烦了。为了统计访问量，要频繁创建SET集合对象。



![image-20220222174354773](Redis.assets/image-20220222174354773.5ade9e1a.png)

Redis实现HyperLogLog算法，HyperLogLog 这个数据结构的发明人 是Philippe Flajolet（菲利普·弗拉若莱）教授。Redis 在 2.8.9 版本添加了 HyperLogLog 结构。

#### [#](https://www.ydlclass.com/doc21xnv/frame/redis/#_8-2-uv计算示例)8.2 UV计算示例

```bash
node1.ydlclass.cn:6379> help @hyperloglog      

增加：
PFADD  key element [element ...]   
summary:  Adds the specified elements to the specified HyperLogLog.   
since:  2.8.9      

计数：
PFCOUNT  key [key ...]   
summary:  Return the approximated cardinality（基数） of the set(s) observed by the HyperLogLog at  key(s).   
since:  2.8.9      

合并：
PFMERGE  destkey sourcekey [sourcekey ...]   
summary:  Merge N different HyperLogLogs into a single one.   
since:  2.8.9  
```



Redis集成的HyperLogLog使用语法主要有pfadd和pfcount，顾名思义，一个是来添加数据，一个是来统计的。为什么用**pf**？是因为HyperLogLog 这个数据结构的发明人 是Philippe Flajolet教授 ，所以用发明人的英文缩写，这样容易记住这个语法了。

下面我们通过一个示例，来演示如何计算uv。

```text
node1.ydlclass.cn:6379> pfadd uv user1  
(integer) 1  
node1.ydlclass.cn:6379> keys *  
1) "uv"  
node1.ydlclass.cn:6379> pfcount uv  
(integer) 1  
node1.ydlclass.cn:6379> pfadd uv user2  
(integer) 1  
node1.ydlclass.cn:6379> pfcount uv  
(integer) 2  
node1.ydlclass.cn:6379> pfadd uv user3  
(integer) 1  
node1.ydlclass.cn:6379> pfcount uv  
(integer) 3  
node1.ydlclass.cn:6379> pfadd uv user4  
(integer) 1  
node1.ydlclass.cn:6379> pfcount uv  
(integer) 4  
node1.ydlclass.cn:6379> pfadd uv user5 user6  user7 user8 user9 user10  
(integer) 1  node1.ydlclass.cn:6379> pfcount uv  
(integer) 10  
```



HyperLogLog算法一开始就是为了大数据量的统计而发明的，所以很适合那种数据量很大，然后又没要求不能有一点误差的计算，HyperLogLog 提供不精确的去重计数方案，虽然不精确但是也不是非常不精确，标准误差是 0.81%，不过这对于页面用户访问量是没影响的，因为这种统计可能是访问量非常巨大，但是又没必要做到绝对准确，访问量对准确率要求没那么高，但是性能存储方面要求就比较高了，而HyperLogLog正好符合这种要求，不会占用太多存储空间，同时性能不错

**pfadd**和**pfcount**常用于统计，需求：假如两个页面很相近，现在想**统计这两个页面的用户访问量**呢？这里就可以用**pfmerge**合并统计了，语法如例子：

```text
node1.ydlclass.cn:6379> pfadd page1 user1 user2  user3 user4 user5  
(integer) 1  
node1.ydlclass.cn:6379> pfadd page2 user1 user2  user3 user6 user7  
(integer) 1  
node1.ydlclass.cn:6379> pfmerge page1+page2  page1 page2  
OK  
node1.ydlclass.cn:6379> pfcount page1+page2  
(integer) 7
```



#### [#](https://www.ydlclass.com/doc21xnv/frame/redis/#_8-3-hyperloglog为什么适合做大量数据的统计)8.3 HyperLogLog为什么适合做大量数据的统计

- Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定的、并且是很小的。
- 在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。
- 但是，因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素。

什么是基数？

比如：数据集{1, 3, 5, 7, 5, 7, 8}，那么这个数据集的基数集{1, 3, 5, 7, 8}，**基数（不重复元素）为5**。基数估计就是在误差可接受的范围内，快速计算基数。





# Redis进阶

**课程目标**(面试、运维)

- 能够理解Redis的持久化

- 能够理解Redis的主从复制架构
- 能够理解Redis的Sentinel架构
- 能够理解Redis cluster集群架构
- redis缓存常见面试题
- 常见问题

# 第一章 Redis的持久化

----

















>reids.conf





```text
注释掉  使redis 可以外部访问
#bind 192.168.245.131

#no  守护线程fang'sh
protected-mode no


port 6379


tcp-backlog 511


timeout 0


tcp-keepalive 300



daemonize yes

pidfile /var/run/redis_6379.pid


loglevel notice

logfile "/export/server/redis-stable/log/redis.log" 




databases 16

proc-title-template "{title} {listen-addr} {server-mode}"

stop-writes-on-bgsave-error yes

rdbcompression yes


rdbchecksum yes


dbfilename dump.rdb


rdb-del-sync-files no


dir /export/server/redis-stable/data



replica-serve-stale-data yes

replica-read-only yes


repl-diskless-sync yes


repl-diskless-sync-delay 5


repl-diskless-sync-max-replicas 0


repl-diskless-load disabled


repl-disable-tcp-nodelay no



lazyfree-lazy-eviction no
lazyfree-lazy-expire no
lazyfree-lazy-server-del no
replica-lazy-flush no


lazyfree-lazy-user-del no


lazyfree-lazy-user-flush no


oom-score-adj no



oom-score-adj-values 0 200 800


disable-thp yes



appendonly no



appendfilename "appendonly.aof"



appenddirname "appendonlydir"




appendfsync everysec

no-appendfsync-on-rewrite no


auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb


aof-load-truncated yes


aof-use-rdb-preamble yes

aof-timestamp-enabled no


slowlog-log-slower-than 10000

slowlog-max-len 128

latency-monitor-threshold 0


notify-keyspace-events ""

hash-max-listpack-entries 512
hash-max-listpack-value 64


list-max-listpack-size -2


list-compress-depth 0

set-max-intset-entries 512

zset-max-listpack-entries 128
zset-max-listpack-value 64


hll-sparse-max-bytes 3000


stream-node-max-bytes 4096
stream-node-max-entries 100


activerehashing yes


client-output-buffer-limit normal 0 0 0
client-output-buffer-limit replica 256mb 64mb 60
client-output-buffer-limit pubsub 32mb 8mb 60


hz 10


dynamic-hz yes


aof-rewrite-incremental-fsync yes


rdb-save-incremental-fsync yes

jemalloc-bg-thread yes


```

