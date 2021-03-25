---
title: "Redis"
date: 2021-03-18T11:54:16+05:30
weight: 1
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



#### Create

`rpush` : It will create the list. If it already exists, then it will append the items to the list

#### Fetch 

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



#### Update



1. `lpush` : append elements to the left of the list
2. `rpush` : append elements to the right of the list



#### Delete



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



#### Create/Update : 

1. `hmset/hset` : create the hash/ updates the hash



Fetch : 

1. `hgetall` : Fetch all the field values 
2. `hget` : Fetch only one field at a time
3. `hmget` : Fetch multiple fields at a time 
4. `hexists` : Check if a field exists in a hash

5. `hvals` : Gives all the values of a key
6. `hkeys` : Gives all the fields of a key



#### Delete : 

1. `hdel`

```foo
127.0.0.1:6379> hset details name "Patrick Jane" alias Mentalist location Sacramento country "United States"
(integer) 4
127.0.0.1:6379> hgetall details
1) "name"
2) "Patrick Jane"
3) "alias"
4) "Mentalist"
5) "location"
6) "Sacramento"
7) "country"
8) "United States"
127.0.0.1:6379> hget details name
"Patrick Jane"
127.0.0.1:6379> hget details alias
"Mentalist"
127.0.0.1:6379> hmget details alias location
1) "Mentalist"
2) "Sacramento"
127.0.0.1:6379> hvals details
1) "Patrick Jane"
2) "Mentalist"
3) "Sacramento"
4) "United States"
127.0.0.1:6379> hkeys details
1) "name"
2) "alias"
3) "location"
4) "country"
127.0.0.1:6379> hset details name Leonard city Pasadena
(integer) 1
127.0.0.1:6379> hgetall details
 1) "name"
 2) "Leonard"
 3) "alias"
 4) "Mentalist"
 5) "location"
 6) "Sacramento"
 7) "country"
 8) "United States"
 9) "city"
10) "Pasadena"
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

4
{b'name': b'Patrick Jane', b'alias': b'Mentalist', b'location': b'Sacramento', b'country': b'United States'}
b'Patrick Jane'
True
[b'Patrick Jane', b'Sacramento']
[b'Patrick Jane', b'Mentalist', b'Sacramento', b'United States']
[b'name', b'alias', b'location', b'country']
1
{b'name': b'Leonard', b'alias': b'Mentalist', b'location': b'Sacramento', b'country': b'United States', b'city': b'Pasadena'}
```







































