# Redis

data types:

Strings

Hashes:

```
redis 127.0.0.1:6379> HMSET user:1 username tutorialspoint password 
tutorialspoint points 200 
OK 
redis 127.0.0.1:6379> HGETALL user:1  
1) "username" 
2) "tutorialspoint" 
3) "password" 
4) "tutorialspoint" 
5) "points" 
6) "200"
```

 Lists:

```
redis 127.0.0.1:6379> lpush tutoriallist redis 
(integer) 1 
redis 127.0.0.1:6379> lpush tutoriallist mongodb 
(integer) 2 
redis 127.0.0.1:6379> lpush tutoriallist rabitmq 
(integer) 3 
redis 127.0.0.1:6379> lrange tutoriallist 0 10  

1) "rabitmq" 
2) "mongodb" 
3) "redis"
```

Sets:

```
redis 127.0.0.1:6379> sadd tutoriallist redis 
(integer) 1 
redis 127.0.0.1:6379> sadd tutoriallist mongodb 
(integer) 1 
redis 127.0.0.1:6379> sadd tutoriallist rabitmq 
(integer) 1 
redis 127.0.0.1:6379> sadd tutoriallist rabitmq 
(integer) 0 
redis 127.0.0.1:6379> smembers tutoriallist  

1) "rabitmq" 
2) "mongodb" 
3) "redis" 
```

Sorted Sets:

```
redis 127.0.0.1:6379> zadd tutoriallist 0 redis 
(integer) 1 
redis 127.0.0.1:6379> zadd tutoriallist 0 mongodb 
(integer) 1 
redis 127.0.0.1:6379> zadd tutoriallist 0 rabitmq 
(integer) 1 
redis 127.0.0.1:6379> zadd tutoriallist 0 rabitmq 
(integer) 0 
redis 127.0.0.1:6379> ZRANGEBYSCORE tutoriallist 0 1000  

1) "redis" 
2) "mongodb" 
3) "rabitmq" 
```