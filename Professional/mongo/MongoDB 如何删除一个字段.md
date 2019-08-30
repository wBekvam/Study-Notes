### MongoDB 如何删除一个字段和修改字段名

##### 1、使用update命令删除一个字段

**update命令**

update命令格式：

db.collection.update(criteria,objNew,upsert,multi)

参数说明：

criteria：查询条件

objNew：update对象和一些更新操作符

upsert：如果不存在update的记录，是否插入objNew这个新的文档，true为插入，默认为false，不插入。

multi：默认是false，只更新找到的第一条记录。如果为true，把按条件查询出来的记录全部更新。

```
//例如要把User表中address字段删除
db.User.update({},{$unset:{'address':''}},false, true)
```



#### 2、使用update命令修改一个字段名称

db.集合名称.update({}, {$rename:{"旧键名称":"新键名称"}}, false, true)

#### 参数提示： 

###### 第一个false表示：可选，这个参数的意思是，如果不存在update的记录，true为插入新的记录，默认是false，不插入。 

###### 第二个true表示：可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。