---
title: "Redis"
date: 2021-03-18T11:40:33+05:30
# chapter: true
weight: 3
pre: "<b>3. </b>"
---



* Redis is an open source, in-memory data structure store which supports doing strings, hashes, lists, sets, sorted sets, bitmaps, hyperloglogs and geospatial indexes.
* It has high availability, replication and automatic partitioning so it is a good option for data persistence
* Redis is written in ANSI C.



## Strings



1. `set` : create string 
2. `get` : fetch string, `nil` if it doesn't exists
3. `append` : append to the existing string variable
4. `del` : delete string
5. `exists` : returns 1 if string exists else 0
6. `incr` : increment string by 1. It works on if string is actually an integer. It doesn't work with float or alphanumeric character.
7. `decr` : decrement by 1. It works on if string is actually an integer. It doesn't work with float or alphanumeric character.
8. `incrby` : increment by a number
9. `decrby` : decrement by a number
10. `strlen` : returns length of string
11. `getrange` : returns a slice of the string. Negative indexing also works just like python
12. `setrange` : set a value of a string over a range starting from an offset
13. *If you are setting a a value of a string which contains spaces then it has to be in quotes otherwise it'll throw an error. If it doesn't have spaces then quotes are optional.*



```foo
127.0.0.1:6379> set a 5
OK
127.0.0.1:6379> get a
"5"
127.0.0.1:6379> append a 2
(integer) 2
127.0.0.1:6379> get a
"52"
127.0.0.1:6379> exists a
(integer) 1
127.0.0.1:6379> incr a
(integer) 53
127.0.0.1:6379> incrby a 100
(integer) 153
127.0.0.1:6379> get a
"153"
127.0.0.1:6379> decr a
(integer) 152
127.0.0.1:6379> decrby a 100
(integer) 52
127.0.0.1:6379> get a
"52"
127.0.0.1:6379> strlen a
(integer) 2
127.0.0.1:6379> del a
(integer) 1
127.0.0.1:6379> exists a
(integer) 0
127.0.0.1:6379> get a
(nil)
127.0.0.1:6379> 
```

```foo
127.0.0.1:6379> set a "This is a string"
OK
127.0.0.1:6379> getrange a 0 3
"This"
127.0.0.1:6379> getrange a -6 -1
"string"
127.0.0.1:6379> setrange a 0 "That"
(integer) 16
127.0.0.1:6379> get a
"That is a string"
127.0.0.1:6379> set b what is this
(error) ERR syntax error
127.0.0.1:6379> 
```



#### Python Implementation

```python
import redis

r = redis.Redis()
print(r.set('a','5'))
print(r.get('a'))
print(r.append('a','2'))
print(r.get('a'))
print(r.exists('a'))
print(r.incr('a'))
print(r.incrby('a',100))
print(r.get('a'))
print(r.decr('a'))
print(r.decrby('a',100))
print(r.get('a'))
print(r.strlen('a'))
print(r.delete('a'))
print(r.exists('a'))
print(r.get('a'))

print(r.set('a',"This is a string"))
print(r.getrange('a',0,3))
print(r.getrange('a',-6,-1))
print(r.setrange('a',0,'That'))
print(r.get('a'))
```

```foo
OUTPUT : 

True
b'5'
2
b'52'
1
53
153
b'153'
152
52
b'52'
2
1
0
None
True
b'This'
b'string'
16
b'That is a string'
```



## Lists



Create

`rpush` : It will create the list. If it already exists, then it will append the items to the list

Fetch 

1. `lrange` : 

    To fetch the whole list

    ```foo
    lrange list_name 0 -1
    ```

    To fetch first 5 items

    ```foo
    lrange list_name 0 4
    ```

    Unlike python, it does include the last item of the mentioned index.

    

    To fetch item of a specific index, if the index doesn't exist it'll give empty array

    ```foo
    lrange list_name 4 4
    ```

2. `llen` : gives the length of the list

3. `lindex` : gives the element by its index

    ```foo
    lindex a 2
    ```



Update



1. `lpush` : append elements to the left of the list
2. `rpush` : append elements to the right of the list



Delete



1. `lpop` : remove elements from the left of the list.

    ```foo
    lpop a
    ```

    By default it only removes single element from the list

    ```foo
    lpop a 2
    ```

    If we mention the count, it will remove that much elements from the list.

2. `rpop` : remove elements from the right of the list

    ```foo
    rpop a
    ```

    By default it only removes single element from the list

    ```foo
    rpop a 2
    ```

    If we mention the count, it will remove that much elements from the list.



#### Python Implementation



```python
import redis
r = redis.Redis()

print(r.rpush('a',*[1,2,3]))
print(r.lrange('a',0,-1))
print(r.lrange('a',0,2))
print(r.lrange('a',2,2))
print(r.llen('a'))
print(r.lindex('a',2))
print(r.lpush('a',*[-1,0]))
print(r.rpush('a',*[4,5,6]))
print(r.lpop('a'))
print(r.rpop('a'))
```

```foo
OUTPUT : 

4
[b'2', b'1', b'2', b'3']
[b'2', b'1', b'2']
[b'2']
4
b'2'
6
9
b'0'
b'6'
```



Python implementation of rpop and lpop only deletes one element at a time for some reason



## Hashes



Create/Update : 

1. `hmset/hset` : create the hash/ updates the hash



Fetch : 

1. `hgetall` : Fetch all the field values 
2. `hget` : Fetch only one field at a time
3. `hmget` : Fetch multiple fields at a time 
4. `hexists` : Check if a field exists in a hash

5. `hvals` : Gives all the values of a key
6. `hkeys` : Gives all the fields of a key



Delete : 

1. `hdel`

```foo
127.0.0.1:6379> hmset details name rs alias albatross location Neuss country Germany
OK
127.0.0.1:6379> hgetall details
1) "name"
2) "rs"
3) "alias"
4) "albatross"
5) "location"
6) "Neuss"
7) "country"
8) "Germany"
127.0.0.1:6379> hget details name
"rs"
127.0.0.1:6379> hexists details alias
(integer) 1
127.0.0.1:6379> hmget details name location
1) "rs"
2) "Neuss"
127.0.0.1:6379> hvals details
1) "rs"
2) "albatross"
3) "Neuss"
4) "Germany"
127.0.0.1:6379> hkeys details
1) "name"
2) "alias"
3) "location"
4) "country"
127.0.0.1:6379> hmset details name robin city Dusseldorf
OK
127.0.0.1:6379> hgetall details
 1) "name"
 2) "robin"
 3) "alias"
 4) "albatross"
 5) "location"
 6) "Neuss"
 7) "country"
 8) "Germany"
 9) "city"
10) "Dusseldorf"
127.0.0.1:6379> hdel details name
(integer) 1
127.0.0.1:6379> hgetall details
1) "alias"
2) "albatross"
3) "location"
4) "Neuss"
5) "country"
6) "Germany"
7) "city"
8) "Dusseldorf"
```



#### Python Implementation

```python
import redis

r = redis.Redis()

info = {
   'name' : 'rs',
   'alias' : 'albatross',
   'location' : 'Neuss',
   'country' : 'Germany'
}

print(r.hset("details",mapping=info))
print(r.hgetall("details"))
print(r.hget("details","name"))
print(r.hexists("details","alias"))
print(r.hmget("details",["name","location"]))
print(r.hvals("details"))
print(r.hkeys("details"))
print(r.hset("details",mapping = {"name" : "robin", "city" : "Dusseldorf"}))
print(r.hgetall("details"))
```

```foo
OUTPUT : 

0
{b'alias': b'albatross', b'location': b'Neuss', b'country': b'Germany', b'city': b'Dusseldorf', b'name': b'rs', b'surname': b'siwach'}
b'rs'
True
[b'rs', b'Neuss']
[b'albatross', b'Neuss', b'Germany', b'Dusseldorf', b'rs', b'siwach']
[b'alias', b'location', b'country', b'city', b'name', b'surname']
0
{b'alias': b'albatross', b'location': b'Neuss', b'country': b'Germany', b'city': b'Dusseldorf', b'name': b'robin', b'surname': b'siwach'}
```





## Sets



Create : 

1. `sadd` : create/append

Fetch : 

1. `smembers` : fetch the elements
2. `scard` : Returns total no of elements

Diff

1. `sdiff` : gives difference of set
2. `sdiffstore` : stores difference in third set

Union

1. `sunion` : gives union of sets
2. `sunionstore` : stores union in third set

Intersection

1. `sinter`  : gives intersection of the sets
2. `sinterstore` : stores intersection into the third set

Delete

1. `srem`  : removes element from the set

Move

1. `smove` : moves element from one set to another





```foo
127.0.0.1:6379> sadd myset 10 20 30 40
(integer) 4
127.0.0.1:6379> smembers myset
1) "10"
2) "20"
3) "30"
4) "40"
127.0.0.1:6379> sadd myset 30
(integer) 0
127.0.0.1:6379> smembers myset
1) "10"
2) "20"
3) "30"
4) "40"
127.0.0.1:6379> sadd myset 50
(integer) 1
127.0.0.1:6379> smembers myset
1) "10"
2) "20"
3) "30"
4) "40"
5) "50"
127.0.0.1:6379> scard myset 
(integer) 5
127.0.0.1:6379> sadd myset2 40 50 60 70 80
(integer) 5
127.0.0.1:6379> smembers myset2
1) "40"
2) "50"
3) "60"
4) "70"
5) "80"
127.0.0.1:6379> sdiff myset myset2
1) "10"
2) "20"
3) "30"
127.0.0.1:6379> sdiffstore myset3 myset myset2
(integer) 3
127.0.0.1:6379> smembers myset3
1) "10"
2) "20"
3) "30"
127.0.0.1:6379> sunion myset myset2
1) "10"
2) "20"
3) "30"
4) "40"
5) "50"
6) "60"
7) "70"
8) "80"
127.0.0.1:6379> sunionstore myset4 myset myset2
(integer) 8
127.0.0.1:6379> smembers myset4
1) "10"
2) "20"
3) "30"
4) "40"
5) "50"
6) "60"
7) "70"
8) "80"
127.0.0.1:6379> srem myset4 80
(integer) 1
127.0.0.1:6379> smembers myset4
1) "10"
2) "20"
3) "30"
4) "40"
5) "50"
6) "60"
7) "70"
127.0.0.1:6379> srem myset4 70 60 50
(integer) 3
127.0.0.1:6379> smembers myset4
1) "10"
2) "20"
3) "30"
4) "40"
127.0.0.1:6379> smembers myset
1) "10"
2) "20"
3) "30"
4) "40"
5) "50"
6) "60"
7) "70"
8) "80"
127.0.0.1:6379> smembers myset4
1) "10"
2) "20"
3) "30"
4) "40"
127.0.0.1:6379> sinter myset myset4
1) "10"
2) "20"
3) "30"
4) "40"
127.0.0.1:6379> sinterstore myset5 myset myset4
(integer) 4
127.0.0.1:6379> smembers myset5
1) "10"
2) "20"
3) "30"
4) "40"
127.0.0.1:6379> smove myset myset5 80
(integer) 1
127.0.0.1:6379> smembers myset
1) "10"
2) "20"
3) "30"
4) "40"
5) "50"
6) "60"
7) "70"

```



#### Python Implementation

```python
import redis

r = redis.Redis()

print(r.sadd('myset','10'))
print(r.sadd('myset','20'))
print(r.sadd('myset','30'))
print(r.sadd('myset','40'))
print(r.smembers('myset'))
print(r.sadd('myset',30))
print(r.smembers('myset'))
print(r.sadd('myset',50))
print(r.smembers('myset'))
print(r.scard('myset'))
print(r.sadd('myset2','40'))
print(r.sadd('myset2','50'))
print(r.sadd('myset2','60'))
print(r.sadd('myset2','70'))
print(r.sadd('myset2','80'))
print(r.smembers('myset2'))
print(r.sdiff('myset','myset2'))
print(r.sdiffstore('myset3','myset','myset2'))
print(r.smembers('myset3'))
print(r.sunion('myset','myset2'))
print(r.sunionstore('myset4','myset','myset2'))
print(r.smembers('myset4'))
print(r.srem('myset4',80))
print(r.smembers('myset4'))
print(r.sinter('myset','myset4'))
print(r.sinterstore('myset5','myset','myset4'))

```

```foo
OUTPUT : 

1
1
1
1
{b'20', b'30', b'40', b'10'}
0
{b'20', b'30', b'40', b'10'}
1
{b'20', b'30', b'40', b'50', b'10'}
5
1
1
1
1
1
{b'60', b'40', b'80', b'70', b'50'}
{b'20', b'30', b'10'}
3
{b'20', b'30', b'10'}
{b'20', b'30', b'40', b'60', b'80', b'70', b'50', b'10'}
8
{b'20', b'30', b'40', b'60', b'80', b'70', b'50', b'10'}
1
{b'20', b'30', b'40', b'60', b'70', b'50', b'10'}
{b'20', b'30', b'40', b'50', b'10'}
5
```





## Sorted Sets



Each set member is assigned a numerical value score. Members are sorted on the basis of score.





1. `zadd` : Create/append 
2. `zrange` : list members using range
3. `zcard` : returns total no of elements in the sortedset
4. `zcount` : returns no of elements between range 
5. `zrem` : removes element from the set
6. `zrank` : gives index of the member
7. `zrevrank` : provides the reverse rank
8. `zscore` : provides the score of the member
9. `zrangebyscore` : gives elements by score range



Same score can be assigned to be multiple elements.



```foo
127.0.0.1:6379> zadd myset 1 a 2 b 3 c 5 d
(integer) 4
127.0.0.1:6379> zrange myset 0 -1
1) "a"
2) "b"
3) "c"
4) "d"
127.0.0.1:6379> zadd myset 100 e
(integer) 1
127.0.0.1:6379> zrange myset 0 -1
1) "a"
2) "b"
3) "c"
4) "d"
5) "e"
127.0.0.1:6379> zcard myset
(integer) 5
127.0.0.1:6379> zcount myset 1 3
(integer) 3
127.0.0.1:6379> zcount myset 1 101
(integer) 5
127.0.0.1:6379> zcount myset 1 100
(integer) 5
127.0.0.1:6379> zcount myset 1 99
(integer) 4
127.0.0.1:6379> zrem myset b
(integer) 1
127.0.0.1:6379> zrank myset a
(integer) 0
127.0.0.1:6379> zrank myset e
(integer) 3
127.0.0.1:6379> zrevrank myset a
(integer) 3
127.0.0.1:6379> zrevrank myset e
(integer) 0
127.0.0.1:6379> zscore myset a
"1"
127.0.0.1:6379> zscore myset d
"5"
127.0.0.1:6379> zscore myset e
"100"
```



#### Python Implementation



```python
import redis

r = redis.Redis()
d = {
   'a' : 1,
   'b' : 2,
   'c' : 3,
   'd' : 5
}

print(r.zadd('myset',mapping=d))

print(r.zrange('myset',0,-1))
print(r.zadd('myset',mapping={'e' : 100}))
print(r.zrange('myset',0,-1))
print(r.zcard('myset'))
print(r.zcount('myset',1,3))
print(r.zcount('myset',1,101))
print(r.zcount('myset',1,99))
print(r.zrem('myset','b'))
print(r.zrank('myset','a'))
print(r.zrank('myset','e'))
print(r.zrevrank('myset','a'))
print(r.zrevrank('myset','e'))
print(r.zscore('myset','a'))
print(r.zscore('myset','d'))
print(r.zscore('myset','e'))
```

```foo
OUTPUT : 

1
[b'a', b'b', b'c', b'd', b'e']
0
[b'a', b'b', b'c', b'd', b'e']
5
3
5
4
1
0
3
3
0
1.0
5.0
100.0
```

