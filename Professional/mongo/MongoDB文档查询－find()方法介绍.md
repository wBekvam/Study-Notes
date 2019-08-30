# MongoDB文档查询－find()方法介绍

### find

`find`用来查询多条文档

```
Model.find(conditions, [fields], [options], [callback])
conditions <Object> //查询条件
[fields] <Object> //要查询的字段
[options] <Object> //查询配置参数
[callback] <Function> //回调
```

#### 条件查询

条件查询中常用属性

- $or　　　　或关系
- $nor　　　　或关系取反
- $gt　　　　大于
- $gte　　　　大于等于
- $lt　　　　小于
- $lte　　　　小于等于
- $ne　　　　不等于
- $in　　　　在多个值范围内
- $nin　　　　不在多个值范围内
- $all　　　　匹配数组中多个值
- $regex　　　　正则，用于模糊查询
- $size　　　　匹配数组大小
- $maxDistance　　　　范围查询，距离（基于LBS）
- $mod　　　　取模运算
- $near　　　　邻域查询，查询附近的位置（基于LBS）
- $exists　　　　字段是否存在
- $elemMatch　　　　匹配内数组内的元素
- $within　　　　范围查询（基于LBS）
- $box　　　　范围查询，矩形范围（基于LBS）
- $center　　　　范围醒询，圆形范围（基于LBS）
- $centerSphere　　　　范围查询，球形范围（基于LBS）
- $slice　　　　查询字段集合中的元素（比如从第几个之后，第N到第M个元素

如要查询阅读量大于500小于600的文章

```
Article.find({views : {$gte : 500 , $lte : 600}})
```

#### options

参看该[文档](https://github.com/qianjiahao/MongoDB/wiki/MongoDB从零起步走之find上)

#### findById

`findById` 用来通过`id`查询单条文档

```
Model.findById(id, [fields], [options], [callback])
```

#### findOne

`findOne` 用来通过条件查询单条文档



```
Model.findOne(conditions, [fields], [options], [callback])
```

文档查询是MongoDB主要功能之一，在MongoDB中查询使用`find`方法，也可以使用`findOne`方法返回单个文档。相比关系型数据库，MongoDB的查询更为方便，MongoDB的查询提供了：条件查询、语义查询、甚至数组等特定类型的查询。



在MongoDB中，`find()`方法用来查询文档。该方法的返回值是一个表示查询结果的文档子集，其范围是0个文档到整个文档集合。



### 1. 语法结构

```
db.collection.find(query, projection)
```

- `query`，可选。表示集合的查询条件。可指定条件查询参数，或一个空对象`({})`

- ```
  projection
  ```

  ，可选。表示查询时所要返回的字段，省略此参数将返回全部字段。其格式如下：

  - `{ field1: <boolean>, field2: <boolean> ... }`

- 返回值

  返回表示查询文档的游标，即：执行`find()`方法时其返回的文档，实际是对文档引用的一个游标。

  当指定`projection`参数时，返回值仅包含指定的字段和`_id`字段，也可以指定不返回`_id`字段。

  在Mongo shell中查询时，`find()`方法默认返回前20条数据，再次执行将继续向前查询



### 2. 使用示例

#### 查询集合中的所有文档

`find()`方法不指定任何参数，将返回所有的文档的所有字段。如，查询students集合中的所有数据：

```
db.students.find()
```



#### 查询集合中的指定文档

查询集合中指定文档时，需要指定`query`参数。如，查询年龄小于15的学生：

```
db.students.find({age:{$lt:15}})
```



#### 查询集合中的指定文档

`_id`字段是MongoDB集合文档的标识，每个文档的`_id`都是唯一的。创建记录时，可以指定该字段值，如果不集定MongoDB将自动创建一个值。同样，通过该字段查询出来的文档也是唯一的：

```
db.students.find({_id:5})
```



#### 指定查询操作符

`$in`操作符相当于关系型数据库中的`in()`查询，但`$in`操作符指定查询对象是一个数组。如，查询_id为5或为ObjectId("2d93w03akdie834k26cmsa01")的文档：

```
db.students.find(
  {
    _id: { $in: [ 5,  ObjectId("2d93w03akdie834k26cmsa01") ] }
  }
)
```



#### 范围查询

MongoDB同样提供了类型关系型数据库的范围查询功能（`between`），但MongoDB的范围查询更为简单。如，查询年龄在10〜15之间的学生：

```
db.students.find({age: {$gt:10, $lt:15}})
```



#### 指定查询字段

不指定`projection`参数时，默认会返回文档中的全部字段。当我们只需要查询部分字段时，我们可以指定`projection`参数。如：

```
db.products.find( { qty: { $gt: 25 } }, { item: 1, qty: 1 } )
```

返回值：

```
{ "_id" : 11, "item" : "pencil", "qty" : 50 }
{ "_id" : ObjectId("50634d86be4617f17bb159cd"), "item" : "bottle", "qty" : 30 }
{ "_id" : ObjectId("50634dbcbe4617f17bb159d0"), "item" : "paper", "qty" : 100 }
```



#### 明确指定`_id`字段

`_id`字段不同于其字段，其不明确指定时默认会返回，所在，在不需要此字段时，需要明确指定。如下：

```
db.products.find( { qty: { $gt: 25 } }, { _id: 0, qty: 0 } )
```

返回值：

```
{ "item" : "pencil", "type" : "no.2" }
{ "item" : "bottle", "type" : "blue" }
{ "item" : "paper" }
```

