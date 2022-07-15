SET key1 "Hello"
DEL key1 key2 key3

for key in `echo 'KEYS user*' | redis-cli | awk '{print $1}'`
 do echo DEL $key
done | redis-cli

redis-cli FLUSHDB
redis-cli FLUSHALL

redis-cli -h {hostname_IP} -p {port} -s {socket} -a {password} FLUSHDB
redis-cli -h {hostname_IP} -p {port} -s {socket} -a {password} FLUSHALL

redis-cli FLUSHDB ASYNC
redis-cli FLUSHALL ASYNC
