+++
title = 'Magic Api'
date = 2023-11-09T06:20:28+08:00
draft = true
+++
## magic api 中使用 redis数据多时

使用redis.setex(key, exp, val)时，val只能存String  
常用的 ```redis.setex(key, exp, val::stringify)```  
取出来时又要用 ```redis.get(key)::json```  
用来存一个list时，由于数据比较多，所以这个操作要花掉几秒。  
而使用这个主要是通过list中item的两个key1和key2是否有一个与关键字keyword  相同  
于是可以把list拆分成多个，按key1的前n个字符来分组  
存入的
```java
import redis;
import '@/xxx/xxx/get_list_all' as getListAll;
var all = getListAll()
var grouped = {}
for(item in all){
    var key1First2 = item.key1.substring(0, 2)
    var key2First2 = item.key1.substring(0, 2)
    if(grouped[key1First]==null){
        grouped[key1First] = []
    }
    if(grouped[key2First]==null){
        grouped[key2First] = []
    }
    grouped[key1First].add(item)
    grouped[key2First].add(item)
}
var keyPrefix = 'grouped:'
var exp = 8 * 3600
for(k,v in grouped){
    redis.setex(keyPrefix + k, exp, v::stringify)
}
return {success: true}
```

使用查找时

```java
import redis
var keyword = 'abc'
var keyPrefix = 'grouped:'

var subList = redis.get(keyPrefix + keyword.substring(0, 2))::json

return subList.find(v=>v.key1 == keyword || v.key2 == keyword)
```
