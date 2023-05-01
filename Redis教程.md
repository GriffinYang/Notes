# Redis

Redis作为一个中间件，往往扮演者缓存的作用，有时甚至可以作为消息队列来使用，它相较于MySql这种传统数据库而言，其依靠与运行于内存之中的特性继而使其拥有更快的速度和效能。

## Redis数据类型

### Strings

这是redis最常用的一种数据类型，了其设置和获取都很简单:

```shell
set key value 	#设置一个值为value的名字为key的字符串
get key			#获得一个名字为key的字符串的值，也就是前面声明的value
```

#### 强势的set

我们的set可以为拥有指定key的“对象”设置其值为字符串，如果这个key(对象)已经存在。而且其可以将非字符类型的key所对应的对象转换为字符类型并为之赋值

#### key和value

在redis中我们的key是可以为任何数据，甚至可以是一张图片，而value也是一样。只是它们的大小都不允许超过520MB,而且通常情况下我们使用字符串作为key，这个字符串没有必要太短，也没有必要太长

#### 储存json数据

```shell
set user:01 "\"{'name':'rongxin',age:23}\""
```

此时我们便储存了一个json对象:

```json
"{'name':'rongxin',age:23}"
```

#### 分隔符

当我们的key中包含多个单词时，此时我们需要使用分隔符来实现单词的划分，在redis中我们一般使用**":"**或者**"-"**来实现，如：

```shell
user:name
user-name
```

#### SETNX&MGET

前面我们使用set和and进行了字符串的设置和获取，但是实际上，当我使用了set时，它会声明并为之赋值，如果这个key并不存在，在key存在时其会将原有的值设置为当前值，即使它可能并不是一个字符串。而**使用了SETNX后，我们仅在该key不存在时才会实现数据的存储**。

MGET则是实现同时获得多个字符串的操作:
```shell
 mget user:alpha user:beta
 # 结果
 # 1）"alpha"
 # 2）"beta"
```

### List

#### lpush | rpush

左入组和右入组，分别从左和从右推入数组元素:

```shell
127.0.0.1:6379> lpush work:queue:ids 1001
(integer) 1
127.0.0.1:6379> lpush work:queue:ids 1002
(integer) 2
127.0.0.1:6379> lpush work:queue:ids 1003
(integer) 3
127.0.0.1:6379> rpush work:queue:ids 1004
(integer) 4
127.0.0.1:6379> rpush work:queue:ids 1005
(integer) 5
127.0.0.1:6379> rpush work:queue:ids 1006
(integer) 6
#结果
1	1003
   
2	1002
   
3	1001
   
4	1004
   
5	1005
   
6	1006
```

数组的最大长度为:  2^32 - 1 (4,294,967,295)个元素

#### lpop | rpop

左出组和右出组，分别从左和从右推出数组元素，并返回其值:

```
127.0.0.1:6379> lpop work:queue:ids
"1003"
127.0.0.1:6379> lpop work:queue:ids
"1002"
127.0.0.1:6379> rpop work:queue:ids
"1006"
127.0.0.1:6379> rpop work:queue:ids
"1005"
```

#### llen

获取数组的长度:

```shell
llen work:queue:ids
(integer) 2
```

#### lmove

将一个数组的一个元素移动到另一个数组之中,如我们拥有一个**src[a,b,c]**和一个**target[x,y,z]**。我们使用:

```shell
lmove src target right left
```

此时src最右侧的元素将会被添加到target的最左侧,因此，数组分别变为:**src[a,b]**,**target[c,x,y,z]**

#### lrange

使用该指令可以查看指定范围(**start,end**)的数组的数据，其开始大于结束时，将会返回一个空数组，结束大于最大索引时将会被视为最大索引，其开始和结束均可为负值，其表示自右向左的方向(**默认是自左向右**),但是注意自右向左的第一个元素为-1，也就是说最后一个为-1

**src[a,b]**

```shell
127.0.0.1:6379> lrange src 0 1
1) "a"
2) "b"
127.0.0.1:6379> lrange src 0 2
1) "a"
2) "b"
127.0.0.1:6379> lrange src 0 -1
1) "a"
2) "b"
127.0.0.1:6379> lrange src 0 -2
1) "a"
```

#### ltrim

使用该指令将仅保留起始到结束这个范围内的元素:

```shell
redis> RPUSH mylist "one"
(integer) 1
redis> RPUSH mylist "two"
(integer) 2
redis> RPUSH mylist "three"
(integer) 3
redis> LTRIM mylist 1 -1
"OK"
redis> LRANGE mylist 0 -1
1) "two"
2) "three"
```

#### 阻塞指令blpop&blmove
