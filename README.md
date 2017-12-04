# redis-migration

**redis-migration** is a fast, light-weight migration tool for redis. Just add a new file "redis-migration.c" to the redis sources. We **scale out** our redis clusters by this tool. Please google(baidu) "redis-migration：独创的redis在线数据迁移工具" for more info. Btw, there is a flash for redis migration in source code, 'redis replication flash.pptx'.

redis-migrate是一款超轻量级的redis数据迁移工具，保持redis源码文件不做改动的前提下，只新增加了一个"redis-migration.c"文件，大家可以使用google或者baidu搜索"redis-migration：独创的redis在线数据迁移工具"获取更详细信息。另，源码包里附上了一份redis数据迁移的动画'redis replication flash.pptx'，希望大家喜欢。

Thank you for your use and attention,  i am very very happy ^_^

![image](http://nos.netease.com/knowledge/2c39da89-5b57-4c8c-905a-ed10347bbc76)
## Build

To build redis-migration from source:

    $ git clone https://github.com/helifu/redis-migration.git
    $ cd redis-migration/deps/
    $ chmod +x update-jemalloc.sh
    $ ./update-jemalloc.sh 3.6.0
    $ cd jemalloc
    $ ./configure
    $ make
    $ make install

    $ cd ../../src/
    $ chmod +x mkreleasehdr.sh
    $ ./mkreleasehdr.sh
    $ cd ../
    $ make
    $ make install

## Help

    Usage: redis-migration [options]
           redis-migration -v or --version
           redis-migration -h or --help
           redis-migration -sip: source ip
           redis-migration -sport: source port
           redis-migration -sauth: source password
           redis-migration -dip: destination ip
           redis-migration -dport: destination port
           redis-migration -dauth: destination auth
           redis-migration -rdb: rdb file (snapshot data)
           redis-migration -aof: aof file (delta data)
           redis-migration -client: client num
    Examples:
           ./redis-migration -h
           ./redis-migration -v
           ./redis-migration -sip 192.168.1.1 -sport 6379 -sauth 123456 -dip 192.168.1.2 -dport 6379 -dauth 654321
           ./redis-migration -sip 192.168.1.1 -sport 6379 -sauth 123456 -dip 192.168.1.2 -dport 6379 -dauth 654321 -rdb /tmp/dump.rdb -aof /tmp/appendonly.aof -client 100


## Tips
    Here we use 'twemproxy' as 'destination'.


    1.Dirty data:
        It's very important to empty the database on destination(dip:dport) before migrating data, otherwise 
          there will be dirty data or exception.

    2.DB0:
        Remember that the tool only support db0 database now, but you could comment the code in "redis-migration.c" 
          to support more dbs:
            line 1108   /*if (dbid != 0) {
            line 1109       fprintf(stderr, "Error, RDB file has database:%d\n", dbid);
            line 1110       exit(1);
            line 1111   }*/

    3.Exception:
        1)Server closed the TCP connection: 
![image](http://nos.netease.com/knowledge/06b748b9-d78b-41f4-947b-16285ed525e7)
    
            a.is there any unsupported command for 'twemproxy'?
            b.do you set 'timeout' parameter in redis.conf which belongs to destination's redis-server?
            c.network jitter?
    
        2)Timeout:
![image](http://nos.netease.com/knowledge/8675ab14-e1fe-47c1-88b8-046ba8ec3589)

            a.there is a very very large key/value pair: you could increase the 'timeout' value in 'twemproxy' 
              to solve this problem;

        3)Error:
![image](http://nos.netease.com/knowledge/e3e4a869-c087-483b-a6af-9c98f2c3f085?imageView&thumbnail=980x0)

            a.there is repeated data from multiple sip, such as key1 in sip1 and sip2.
            b.dirty data: do you empty the redis-serveres of 'destination'?
