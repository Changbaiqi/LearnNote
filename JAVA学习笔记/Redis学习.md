---
title: Redis学习
date: 2023-02-24 09:46:09
author: 长白崎
categories:
  - ["Redis"]
tags:
  - "Redis"
---



# Redis学习

---

## Redis安装流程

---

下载Redis包

```shell
wget https://download.redis.io/releases/redis-7.2.0.tar.gz
```



解压文件

```shell
 tar -zxvf redis-7.2.0.tar.gz
```

进入redis安装目录

```shell
cd redis-7.2.0
```



1.Redis是开源的数据库，首先如果要运行话先要用gcc编译器编译好之后才能运行。

如果没有gcc编译器的可以使用yum指令

```sh
yum -y install gcc
```

2.然后就可以在Redis安装包的根目录下执行

```sh
make
```

指令，这样就会自动编译Redis的源代码。

让后就可以在redis根目录和根目录下的src目录下各执行一遍

```sh
make install
```

这样就会将redis的指令全部copy映射到Linux的bin目录，到时候在任何目录下都可以直接执行相关的指令操作redis了。

编译完后就可以直接使用

```sh
redis-server #运行
redis-server & #后台运行
redis-cli shutdown  #停止redis
redis-server -h [IP地址] -p [端口号] #可以用来指定对应的非默认redis启动
redis-cli -h [IP地址] -p [端口号] shutdown #和上面的意思差不多，只不过换成了停止redis

redis-cli  #使用redis自带的客户端连接redis
redis-cli -h [IP地址] -p [端口号]
```



## 将Redis设置成开启自启

---

1.新建文件

```shell
vi /etc/systemd/system/redis.service
```

2.进入之后将以下信息复制进去：（注意ExecStart的内容为你们自己的redis.conf文件的路径）

```shell
[Unit]
Description=redis-server
After=network.target

[Service]
#Type=forking
ExecStart=/usr/local/bin/redis-server /usr/local/redis-7.2.0/redis.conf
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

之后便保存退出。

1. 退出之后要让文件生效

   ```shell
   systemctl daemon-reload
   systemctl enable redis
   ```

2. 然后重启redis

   ```shell
   systemctl start redis
   ```

3. 查看redis状态

   ```shell
   systemctl status redis
   ```



## 如何测试redis服务的性能：

---

执行指令

```sh
redis-benchmark
```

## 查看redis服务是否正常运行：

---

ping		如果正常------pong（前提是在redis-cli连接后）

## 查看redis服务器的统计信息

---

```sh
info # 查看redis服务的所有统计信息
info [信息字段] # 查看redis服务器的指定的统计信息，如info Replication
```

redis的数据库实例：

作用类似于mysql的数据库实例，redis中的数据库实例只能由redis服务来创建和维护，开发人员不能修改和自行创建数据库实例；默认情况下：redis会自动创建16个数据库实例，并且给这些数据库实例进行编号，从0开始，一直到15，使用时通过编号来使用数据库；可以通过配置文件，指定redis自动创建的数据库个数：redis的每一个数据库实例本身占用的存储空间是很少的，所以也不造成存储空间的太多浪费。

​		默认情况下，redis客户端连接的是编号是0的数据库实例

## Redis常用指令：

---

```sh
select [index] #index表示某个数据库的编号，这个指令用来选择数据库
set [key] [value]  #往数据库内添加值
get [key]  #获取对应key的值
dbsize  #查看当前数据库中key的数目，返回当前数据库的key的数量
keys *  #查看当前数据库实例所有的key
flushdb #清空当前库的所有数据
flushall  #清空所有数据库的数据（清空整个Redis服务的数据），谨慎使用！！！
config get *  #查看redis中所有的配置信息
config get parameter #查看redis中的指定的配置信息
```





## Redis的特点

---

### 1. 支持数据持久化

Redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用。

### 2. 支持多种数据结构

Redis不仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。

### 3. 支持数据备份

Redis支持数据的备份，即master-slave模式的数据备份



### A、字符串类型string

字符串类型是Redis中最基本的数据结构，它能存储任何类型的数据，包括二进制数据，序列化后的数据，JSON化的对象甚至是一张图片。最大512M。

| key      | value      |
| -------- | ---------- |
| username | 张三和李四 |



### B、列表类型 list

Redis列表是简单的字符串列表，按照插入顺序排序，元素可以重复。你可以添加一个元素到列表的头部（左边）或者尾部（右边），底层是个链表结构。

| key    | value            |
| ------ | ---------------- |
| reqion | 北京  上海  天津 |



### C、集合类型set

Redis的Set是string类型的无序无重复集合。

<table>
    <tr>
        <th>key</th>
        <th>value</th>
    </tr>
    <tr>
        <td rowspan="4">framework</td>
    </tr>
    <tr>
        <td>spring</td>
    </tr>
    <tr>
        <td>mybatis</td>
    </tr>
    <tr>
        <td>struts</td>
    </tr>
</table>



### D、哈希类型hash

Redis hash 是一个string类型的field和value的映射表，hash特表适合用于存储对象。

| key    | loginuser |
| ------ | --------- |
| field  | value     |
| uname  | 张三      |
| times  | 5         |
| region | 北京      |



### E、有序集合类型zset（sorted set）

Redis有序集合zset和集合set一样也是string类型元素的集合，且不允许重复的成员。不同的是zset的每个元素都会关联一个分数（分数可以重复），redis通过分数来为集合中的成员进行从小到大的排序。

<table>
    <tr>
        <th>key</th>
        <th>value score</th>
    </tr>
    <tr>
        <td rowspan="4">salary</td>
    </tr>
    <tr>
        <td>张三 3500</td>
    </tr>
    <tr>
        <td>李四 5000</td>
    </tr>
    <tr>
        <td>周丽 8000</td>
    </tr>
</table>



## Redis的Key的操作命令

---

### 语法：keys pattern

作用：查找所有符合模式pattern的key，pattern可以使用通配符。

* *：表示0或多个字符，例如：keys *查询所有的key。
* ?：表示单个字符，例如：wo?d，匹配 word，wood
* []：表示选择[]内的一个字符，例如 wo[or]d，匹配word，wood，不匹配wold，woord



### 语法：exists key

作用：判断key在数据库中是否存在。

* exists key：如果存在返回1，如果不存在则返回0
* exists key [key key ...]：返回值是存在的key的数量



### 语法：move key index

作用：移动指定key到指定的数据库实例

* move key index：将当前库的指定的key移动到了index库内



### 语法：ttl

作用：查看指定key的剩余生存时间，ttl为time to live的简称

* ttl key：如果key不存在则返回-2，如果key没有设置生存时间，返回-1



### 语法：expire key seconds

作用：设置key的最大生存时间：expire key seconds

* expire k2 20：给k2设置20秒生存时间，超过20秒k2自动删除



### 语法：type key

作用：查看指定key的数据类型



### 语法：rename key newkey

作用：重命名指定的key



### 语法：del key [key key ...]

作用：删除指定key，返回值是返回实际删除 的数量。



## Redis中有关string类型数据的操作命令：

---

### 语法：set key value

作用：添加一条数据



### 语法：get key

作用：从Redis中获取string类型的数据，如果key已经存在了则后来的value会把以前的value覆盖掉



### 语法：append key value

作用：追加字符串，返回追加后的字符串长度，如果key不存在那么作用就类似于set新创建一个key并追加value值。 



### 语法：strlen key

作用：获取key对应value的字符串长度



### 语法：incr key

作用：将字符串数值进行加1运算，要求key所表示value必须是数值，否则报错。

* incr zsage：返回加1运算之后的数据，如果key不存在，首先设置一个key，值初始化为0，然后进行incr运算。



### 语法：decr key

作用：将字符串数值进行减1运算，要求key所表示value必须是数值，否则报错。

* decr zsage：返回减1运算之后的数据，如果key不存在，首先设置一个key，值初始化为0，然后进行decr运算。



### 语法：incrby key offset

作用：将字符串数值进行加offset运算，返回加offset运算之后的数据，要求key所表示value必须是数值，否则报错。

* incrby zsage 10：如果key不存在，首先设置一个key，初始化为0，然后进行incrby运算。



### 语法：decrby key sffset

作用：将字符串数值进行减offset运算，返回加offset运算之后的数据，要求key所表示value必须是数值，否则报错。

* decrby zsage 10：如果key不存在，首先设置一个key，初始化为0，然后进行decrby运算。



### 语法： getrange key startIndex endIndex

作用：获取字符串中的子字符串，下标自左至右，从0开始，以后往后，最后一个字符的下标是字符串长多-1；字符串中每一个下标也可以是负数，负下标表示自右至左，从-1开始，一次往前，最右边一个字符的下标是-1。

* getrange key startIndex endIndex：获取字符串key中从startIndex到endIndex的字符组成的子字符串



### 语法：setrange key startIndex value

作用：用value覆盖从下标为startIndex开始的字符串



### 语法：setex

作用：设置它最大生命周期

* setex key seconds value



### 语法：setnx key value

作用：设置string类型的数据value到redis数据库中，当key不存在时设置成功，负责将放弃设置



### 语法：mset 键1 值1 键2 值2 键3 值3 ...

作用：批量将string类型的数据设置到redis中



### 语法：mget k1 k2 k3 k4 ...

作用：批量从redis中获取string类型的数据



### 语法：msetnx k1 v1 k2 v2 k3 v3 ...

作用：批量设置string类型的数据value到redis数据库中，当所有key都不存在时设置成功，负责（只要有一个已经存在），则全部放弃设置。 



## Redis中有关列表（List）类型的操作指令

---

Redis列表是简单的字符串列表，按照初入顺序排序，左边（头部））、右边（尾部）或者中间都可以添加元素。链表的操作无论是头或尾效率都极高，但是如果对中间元素进行操作，那效率会大大降低了。

对表类型的数据操作总的思想是通过key和下标操作value，key是数据标识，下标是数据在列表中的位置，value是我们感兴趣的业务数据。

单key-多有序value，一个key对应多个value，多个value之间有顺序，最左侧是表头，最右侧是表尾，每一个元素都有下标，表头元素的下标是0，一次往后排序，最后一个元素下标是列表长度-1；

每一个元素的下标又可以用负数表示，负下标表示从表尾计算，最后一个元素下标用-1表示，元素在列表中的顺序或者下标由放入的顺序来决定。



### 语法：lpush key value [value value value ......]

作用：将一个或者多个值一次插入到列表的表头（左侧）

* lpush ke vlaue [value value ......]



### 语法：lrange key startIndex endIndex

作用：获取指定列表中指定下标区间的元素



### 语法：rpush key value [value value ......]

作用：将一个或者多个值依次插入到列表的表尾（右侧）



### 语法：lpop key

作用：从指定列表头中移除并且返回表头元素



### 语法：rpop key

作用：从指定列表尾中移除并且返回表头元素



### 语法：lindex key index

作用：获取指定列表中指定下标的元素



### 语法：llen key

作用：获取指定列表的长度



### 语法：lrem key value

作用：移除列表中某一位元素（value），跟value相等的。



### 语法：lrem key count value

作用：移除列表中某一些元素，根据count值移除指定列表中跟value相等的数据。当count>0：从列表的左侧移除count个跟value相等的数据；当vount<0：从列表的右侧移除count个跟value相等的数据；当count=0：从列表中移除所有跟value相等的数据



## Redis中有关set类型数据的操作命令

---

Redis的Set是string类型的无序不重复集合。

集合类型的数据操作总的思想是通过key确定集合，key是集合标识，没有下标，只有直接操作业务数据和数据的个数。



### 语法：sadd key value [value value ...]

作用：将一个或者多个元素添加到指定的集合中（添加重复元素时会过滤掉重复元素）

* sadd set01 a b c a：这里实际的只能加入三个元素，也就是a、b、c（不会有重复的数据）



### 语法：smembers key

作用：获取指定集合中所有的元素



### 语法：sismember  key member

作用：判断指定元素（member）在集合（key）中是否存在，存在就返回1，不存在就返回0



### 语法：srem key member [member ......]

作用：移除指定集合中一个或者多个元素，不存在的元素不被忽略，如果存在那么就会返回成功移除的个数。



### 语法：srandmember key [count]

作用：随机获取指定集合中的一个或者多个（count）元素，当count>0：随机获取多个元素之间不能重复；当count<0：随机获取的多个元素之间可能重复



### 语法：spop key [count]

作用：从指定集合中随机移除一个或者多个元素



### 语法：smove source dest member

作用：将指定集合中的指定元素移动到另一个元素

* smove set1 set2 a：移动a元素到set2集合当中



### 语法：sdiff key key [key key ....]

作用：获取指定一个集合中有、但是其它集合中都没有的元素组成的新集合(差集)

* sdiff set1 set2 set3



### 语法：sinter key key key [key key ......]

作用：获取所有指定集合中都有的元素组成的新集合（交集）



### 语法：sunion key key [key key ......]

作用：获取所有指定集合中所有元素组成的大集合



## Redis中有关hash类型数据的操作命令

---

Redis的hash是一个string类型的key和value的映射表，这里的value是一系列的键值对，hash特别适合用于存储对象。

哈希类型的数据操作总的思想是通过key和field操作value，key是数据标识，field是域，value是我们感兴趣的业务数据。



### 语法：hset key filed1 value1 [field2 value2 ...]

作用：将一个或者多个field-value对设置到哈希表中

* hset stu1001 id 1001



### 语法：hget key field

作用：获取指定哈希表中指定field的值

* hget hash id：获取名为hash的hash表中的id的值



### 语法：hmset key filed1 value1 [field2 value2 ......]

作用：批量设置多个filed-value，作用和前面的hset一样



### 语法：hmget key field1 [field2 field3 ......]

作用：批量获取指定哈希表中的field的值



### 语法：hgetall key

作用：获取指定哈希表中所有的field和value



### 语法：hdel key field1 [dield2 field3 ...]

作用：从指定哈希表中删除一个或者多个field



### 语法：hlen key

作用：获取指定哈希表中所有的filed个数 

hlen stu1001：获取key为stu1001的哈希表有多个field。





### 语法：hexists key field

作用：判断指定哈希表中是否存在某一个field，有则返回1，没有则0



### 语法：hkeys key

作用：获取指定哈希表中所有field列表



### 语法：hvals key

作用：获取指定哈希表中所有的value列表



### 语法：hincrby key field int(这里的int只能是整数)

作用：对指定哈希表中指定field值进行加法运算



### 语法：hincrbyfloat key field float

作用：对指定哈希表中指定field值进行浮点数加法运算



### 语法：hsetnx key field value

作用：将一个field-value对设置到哈希表中，当key-field已经存在时，则放弃设置，否则继续设置。



## Redis中有关zset类型数据的操作命令

---

Redis有序集合zset和集合set一样也是string类型元素的集合，且不允许重复的成员

不同的是zset的每个元素都会关联一个分数（分数可以重复），redis通过分数来为集合中的成员进行从小到大的排序。

本质上是集合，所有元素不能重复。既然有序集合中每一个元素都有顺序，那么也都有下标；有序集合中元素的排序规则由列表中元素的排序规则不一样。

  

### 语法：zadd key score member [score member ...]

作用：将一个或多个member及其score值加入有序集合

* zadd zset01 20 z1 z2 50 z3 40 z4



### 语法：zrange key startIndex endIndex [withscores]

作用：获取指定有序集合中指定下标区间的元素，如果加上withscores那么就会显示后面的分数。

* zrange zset01 0 -1



### 语法：zrangebyscore key min max [withscores]

作用：获取指定有序集合中指定分数区间（闭区间）的元素



### 语法：zrem key member [member ......]

作用：删除有指定有序集合中一个或者多个元素

* zrem zset z3 z4



### 语法：zcard key

作用：获取指定有序集合中所有元素的个数



### 语法：zrank key member

作用：获取指定有序集合中指定元素的排名（闭区间）



### 语法：zcount key min max

作用：获取指定有序集合中分数在指定区间内的元素的个数



### 语法：zscore key member

作用：获取指定有序集合中指定元素的分数



### 语法：zrevrank key member

作用：获取指定有序集合中指定元素的排名（按照分数从大到小的排名）



### 语法：zrevrange key startIndex endIndex [WITHSCORES]

作用：查询有序集合，指定区间内的元素。集合成员按score值从大到小来排序；startIndex和endIndex都是从0开始表示第一个元素，1表示第二个元素，以此类推；startIndex和 endIndex都可以取负数，表示从后往前取，返回值：指定区间的成员组成的集合。



## Redis的配置文件

---

在redis根目录下提供redis.conf配置文件；可以配置一些redis服务端运行时的一些参数；如果不适用配置文件，那么redis会按照默认的参数运行；如果使用配置文件，在启动redis服务时候必须要指定redis所使用的配置文件

```sh
redis-server redis.conf  #像这样
```



### Redis配置文件中关于网络的配置：

port：指定redis服务所使用的端口，默认使用6379

bind：配置客户端连接redis服务时，所能使用的ip地址，默认可以使用redis服务所在主机上任何一个ip都可以；一般情况下，都会配置一个ip，而且通常是一个真实的网卡上的IP。

tcp-keepalive：TCP连接保活策略 ，可以通过tcp-keepalive配置项来进行设置，单位为秒，假如设置为60秒，则server端会60秒向连接空闲的客户端发起一次ACK请求，以检查客户端是否已经挂掉，对于无响应的客户端则会关闭其连接。如果设置为0，则不会进行保活检测。



### Redis常规配置：

loglevel：配置日志级别，开发阶段可以设置成debug，生成阶段通常设置为notice或者warning。

logfile：指定日志文件。redis在运行过程中，会输出一些日志信息：默认情况下，这些日志信息会输出到控制台：我们可以使用logfile配置日志文件，使redis把日志信息输出到文件里面。要保证日志文件所在目录必须存在，文件可以不存在。还要在redis启动时指定所使用的配置文件，否则配置不起作用。

databases：配置redis服务默认创建的数据库实例个数，默认值是16



### Redis的安全配置

requirepass：配置Redis的访问密码。默认不配置密码，即访问不需要密码验证。此配置项需要在prorected-mode=yes时起作用。使用密码登录客户端：

```sh
redis-cli -h ip -p 6379 -a pwd
```

redis的持久化：redis提供持久化策略，在适当的时机采用适当手段把内存中的数据持久化到磁盘中，每次redis服务启动时都可以把磁盘上的数据再次加载内存中使用。



### Redis的RDB配置

RDB策略 ：在指定时间间隔内，redis服务执行指定次数的写操作，会自动触发一次持久化操作。

1、save <seconds> <changes>：配置复合的快照触发条件，即Redis在seconds秒内key改变changes次，Redis把快照内的数据保存到了磁盘中一次。默认的策略是：

1分钟内改变1万次

或者5分钟内改变了10次

或者15分钟内改变了1次

如果要禁止Redis的持久化功能，则把所有的save配置都注释掉。

2、stop-writes-on-bgsave-error：当bgsave快照操作出错时停止写数据到磁盘，这样能保证内存珊瑚橘和磁盘数据的一致性，但如果不在乎这种一致性，要在bgsave快照操作出错时继续写操作，这里需要配置为no。

3、rdbcompression：设置对于存储到磁盘中的快照是否进行压缩，设置为yes时，Redis会采用LZF算法进行压缩；如果不想消耗CPU进行压缩的话，可以设置为no，关闭此功能。

4、rdbchecksum：在存储快照以后，还可以让Redis使用CRC4算法来进行数据校验，但这样会消耗一定的性能，如果系统比较在意性能的提升，可以设置为no，关闭此功能。

5、dbfilename：Redis持久化数据生成的文件名，默认是dump.rdb，也可以自己配置。

6、dir：Redis持久化数据生成文件保存的目录，默认是./即redis的启动目录，也可以自己配置。



### Redis的AOF策略：

AOF策略：采用操作日志来记录进行每一次写操作，每次redis服务启动时，都会重新执行一遍操作日志中的指令。redis默认不开启，效率低。

appendonly：配置是否开启AOF策略，默认为no即为关闭

appendfilename：配置操作日志文件

小结：根据数据的特点决定开启哪种持久化策略；一般情况，开启RDB足够了。





## Redis的事务：

事务：把一组数据库命令放在一起执行，保证操作的原子性，要么同时成功，要么同时失败。

Redis的事务：允许在把一组redis数据库命令放在一起执行，把命令进行序列化，然后一起执行，保证部分原子性

1、单独的隔离操作：事务中的所有命令都会序列化、顺序地执行、事务在执行过程中，不会被其它客户端发来的命令请求所打断，除非使用watch命令监控某些键。

2、不保证事务的原子性：redis同一事务中如果一条命令执行失败，其后的命令仍然可能会被执行，redis的事务没有回滚。Redis已经在系统内部进行功能简化，这样可以确保更快的运行速度，因为Redis不需要事务回滚的能力。

### 1）multi：用来标记一个事务的开始。

### 2）exec：用来执行事务队列中所有的命令。

### 3）redis的事务只能保证部分原子性：

​	a）如果一组命令中，有在加入事务队列过程中发生错误的命令，则本事务中所有的命令都不执行，能够保证事务的原子性。

```sh
multi
seta kk vv
set k4 v4
exec
#这里上面所有的命令都不会凑效
```

 b）如果一组命令中，在压入队列过程中正常，但是在执行事务 队列命令时发生了错误，则只会影响发生错误的命令，不会影响其他命令的执行，这时就不能保证事务的原子性。

```sh
multi
set k3 v3
incr k1
set k4 v4
exec
#压入队列的时候这些指令并没有执行，执行的时候会报错，这个时候就只会忽略报错的指令其他指令照常，这时候就不能保证事务的原子性了，只能说部分原子性。
```

### 4）discard：清楚所有已经压入队列中的命令，并且结束整个事务。

```sh
set k5 v5
set k6 v6
discard
```

### 5） watch:监控某一个键，当事务在执行过程中，此键代表的值发生了变化，则本事务放弃执行，否则正常执行。

```sh
set balance 100
set balance2 1000
set version 1

watch version
multi
decrby balance 50
incrby balance2 50
exec
```

### 6）unwatch：放弃监控所有的键。



## Redis消息的发布与订阅

---

redis客户端订阅频道，消息的发布者往频道上发布消息，所有订阅此频道的客户端都能够接受到

### 1）subscribe ch1 ch2 [ch3 ch4 ...]：定义频道

### 2）publish ch1 message：发布消息，这里是往ch1发送message

### 3）psubcribe：订阅一个或者多个频道的消息，频道名支持通配符。

* subscribe news.* 



## Redis的主从复制