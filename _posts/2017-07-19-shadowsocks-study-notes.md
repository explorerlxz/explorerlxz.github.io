---
layout: post
title:  "Learn python from shadowsocks 2.6"
date:   2017-07-19
---



# Testing program


## This line ***config = json.loads(f.read().decode('utf8'), object_hook=_decode_dict)*** in shadowsocks-2.6/shadowsocks/utils.py


### The original code block:

{% highlight python %}
#shadowsocks-2.6/shadowsocks/utils.py
def get_config(is_local):
###################################
#   A lot code here
###################################

       if config_path:
            logging.info('loading config from %s' % config_path)
            with open(config_path, 'rb') as f:#Read configure file
                try:
                    config = json.loads(f.read().decode('utf8'),
                                        object_hook=_decode_dict)	# I don't understand this line
                except ValueError as e:
                    logging.error('found an error in config.json: %s',
                                  e.message)
                    sys.exit(1)

###################################
#   A lot code here
###################################

def _decode_list(data):
    rv = []
    for item in data:
        if hasattr(item, 'encode'):
            item = item.encode('utf-8')
        elif isinstance(item, list):
            item = _decode_list(item)
        elif isinstance(item, dict):
            item = _decode_dict(item)
        rv.append(item)
    return rv


def _decode_dict(data):
    rv = {}
    for key, value in data.items():
        if hasattr(value, 'encode'):
            value = value.encode('utf-8')
        elif isinstance(value, list):
            value = _decode_list(value)
        elif isinstance(value, dict):
            value = _decode_dict(value)
        rv[key] = value
    return rv
{% endhighlight %}


### My test file:


{% highlight python %}
#test.py
import json
import sys

def _decode_dict(data):
    rv = {}
    for key, value in data.items():
        if hasattr(value, 'encode'):
            value = value.encode('utf-8')
        elif isinstance(value, list):
            value = _decode_list(value)
        elif isinstance(value, dict):
            value = _decode_dict(value)
        rv[key] = value
    return rv

def _decode_list(data):
    rv = []
    for item in data:
        if hasattr(item, 'encode'):
            item = item.encode('utf-8')
        elif isinstance(item, list):
            item = _decode_list(item)
        elif isinstance(item, dict):
            item = _decode_dict(item)
        rv.append(item)
    return rv

with open('config.json', 'rb') as f:
    try:
        config = json.loads(f.read().decode('utf8'), object_hook=_decode_dict)
        print(config)
    except ValueError as e:
        logging.error('found an error in config.json: %s', e.message)
        sys.exit(1)

{% endhighlight %}


### Related configure file `config.json`:

```
{
    "server":"my_server_ip",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false,
    "workers": 1
}
```

When I run ***python test.py***, I get the following result:

```
{u'server_port': 8388, u'local_port': 1080, u'workers': 1, u'fast_open': False, u'server': 'my_server_ip', u'timeout': 300, u'local_address': '127.0.0.1', u'password': 'mypassword', u'method': 'aes-256-cfb'}
```

when I run ***python3 test.py***, I will get this result:

```
{'password': b'mypassword', 'server': b'my_server_ip', 'method': b'aes-256-cfb', 'server_port': 8388, 'fast_open': False, 'local_address': b'127.0.0.1', 'workers': 1, 'timeout': 300, 'local_port': 1080}
```






|文件名|功能|备注|
|-----|---|----|
|asyncdns.py|异步处理dns请求|提供远端dns查询|
|common.py|工具函数|进行header和addr数据包处理|
|utils.py|工具函数|实现config配置检查|
|daemon.py|linux守护进程||
|encrypt.py|加密解密|简单易懂，不涉及到底层算法，封装很好|
|eventloop.py|事件循环|实现IO复用，封装成类Eventloop，多平台|
|local.py|客户端||
|server.py|服务端|支持多用户|
|lru_cache.py|缓存|基于LRU的Key-Value字典|
|tcprelay.py|tcp的转达||
|udprelay.py|udp的转达|local <=> client <=> server <=> dest|


### eventloop.py

事件循环类

    使用select、epoll、kqueue等IO复用实现异步处理。
    优先级为epoll\>kqueue\>select
    Eventloop将三种复用机制的add，remove，poll，add_handler，remve_handler接口统一
    程序员只需要使用这些函数即可，不需要处理底层细节。
    当eventloop监测到socket的数据，程序就将所有监测到的socket和事件交给所有handler去处理
    每个handler通过socket和事件判断自己是否要处理该事件，并进行相对的处理：

### udprelay.py / tcprelay.py / asyndns.py

数据包的转发处理

    三个文件分别实现用来处理udp的请求，tcp的请求，dns的查询请求
    将三种请求的处理包装成handler。
    对于tcp，udp的handler，它们bind到特定的端口，并且将socket交给eventloop，并且将自己的处理函数加到eventloop的handlers
    对于dns的handler，它接受来自udp handler和tcp handler的dns查询请求，并且向远程dns服务器发出udp请求
    协议解析和构建用的struct.pack()和struct.unpack()

#### lru_cache.py

实现缓存，英文:Least Recently Used Cache

    先找访问时间_last_visits中超出timeout的所有键
    然后去找_time_to_keys，找出所有可能过期的键
    因为最早访问时间访问过的键之后可能又访问了，所以要_keys_to_last_time
    找出那些没被访问过的，然后删除
    
#### 其他

这个项目有一些比较有趣的代码，目的是兼容python3标准
- 每一个py文件前面都有导入3.x的package:  __future__  ，如果使用print "hello"，会提示出错，因此使用print("hello")
- 给字符串赋值时，总是带有一个前缀'b'，比如：v4addr=b'8.8.4.4'，是为了兼容py3标准，[参考这里](http://stackoverflow.com/questions/6269765/what-does-the-b-character-do-in-front-of-a-string-literal)







## Reference

 - [json — JSON encoder and decoder](https://docs.python.org/2/library/json.html)
 - [shadowsocks-analysis](https://github.com/lixingcong/shadowsocks-analysis)

