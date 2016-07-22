# redis-migration

**redis-migration** is a fast, light-weight migration tool for redis. Just add redis-migration.c to the redis sources. We **scale out** our redis clusters by this tool.
![image](http://nos.netease.com/knowledge/2c39da89-5b57-4c8c-905a-ed10347bbc76)
## Build

To build redis-migration from source:

    $ git clone git@github.com:twitter/twemproxy.git
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
           redis-migration -rdb: rdb file
           redis-migration -aof: aof file
           redis-migration -client: client num
    Examples:
           ./redis-migration -h
           ./redis-migration -v
           ./redis-migration -sip 192.168.1.1 -sport 6379 -sauth 123456 -dip 192.168.1.2 -dport 6379 -dauth 654321
           ./redis-migration -sip 192.168.1.1 -sport 6379 -sauth 123456 -dip 192.168.1.2 -dport 6379 -dauth 654321 -rdb /tmp/dump.rdb -aof /tmp/appendonly.aof -client 100
