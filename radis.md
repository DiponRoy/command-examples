
# redis
<p align="left">
    <a href="https://redis.io/" target="_blank"> <img src="https://cdn.iconscout.com/icon/free/png-256/redis-83994.png" alt="r" width="40" height="40" /> </a>
</p>














redis-cli is a terminal program used to send commands to and read replies from the Redis server
 -  **official**: https://redis.io/docs/manual/cli/


## Radis Cmds
```
redis-cli SET key1 "Hello"
redis-cli GET key1
redis-cli DEL key1

for key in `echo 'KEYS key*' | redis-cli | awk '{print $1}'`
 do echo GET $key
done | redis-cli

for key in `echo 'KEYS key*' | redis-cli | awk '{print $1}'`
 do echo DEL $key
done | redis-cli

redis-cli FLUSHDB
redis-cli FLUSHALL

redis-cli -h {hostname_IP} -p {port} -s {socket} -a {password} FLUSHDB
redis-cli -h {hostname_IP} -p {port} -s {socket} -a {password} FLUSHALL

redis-cli FLUSHDB ASYNC
redis-cli FLUSHALL ASYNC
```
