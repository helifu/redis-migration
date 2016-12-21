# redis-migration

**redis-migration** is a fast, light-weight migration tool for redis. Just add redis-migration.c to the redis sources. We **scale out** our redis clusters by this tool. <http://www.bitstech.net/2016/03/03/redis-migration/>

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

    1.Dirty data:
        It's very import to empty the database on dip:dport before migrating data, otherwise there will be dirty data or exception.

    2.DB0:
        Remember that this tool only support db0 database.

    3.Exception:
        Server closed the TCP connection: 
        ![image](http://nos.netease.com/knowledge/06b748b9-d78b-41f4-947b-16285ed525e7)
    
            a.redis-server 'timeout' parameter in redis.conf?
            b.is there any unsupported command for twemproxy, if dip is twemproxy?
            c.network jitter?
    
        Timeout:
            ![image](http://nos.netease.com/knowledge/8675ab14-e1fe-47c1-88b8-046ba8ec3589)

            a.there is a large key/value: you can modify the 'timeout' parameter for twemproxy if your dip is twemproxy;

        Error:
            ![image](http://nos.netease.com/knowledge/e3e4a869-c087-483b-a6af-9c98f2c3f085?imageView&thumbnail=980x0)

            a.repeated data from multi-sip;
            b.dirty data;
