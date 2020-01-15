# MongoDB记录

### MongoDB--连接

```js
mongodb://[username:password@]host1[:port1][,host2[:port2],...[,hostN[:portN]]][/[database][?options]]
```

- **mongodb://** 这是固定的格式，必须要指定。
- **username:password@** 可选项，如果设置，在连接数据库服务器之后，驱动都会尝试登陆这个数据库
-  **host1** 必须的指定至少一个host, host1 是这个URI唯一要填写的。它指定了要连接服务器的地址。如果要连接复制集，请指定多个主机地址。
- **portX** 可选的指定端口，如果不填，默认为27017
- **/database **如果指定username:password@，连接并验证登陆指定数据库。若不指定，默认打开 test 数据库。
-  **?options** 是连接选项。如果不使用/database，则前面需要加上/。所有连接选项都是键值对name=value，键值对之间通过&或;（分号）隔开

##### 连接一个服务器实例：

```
mongodb://localhost
```

使用用户名fred，密码foobar登录localhost的admin数据库。

```
mongodb://fred:foobar@localhost
```

使用用户名fred，密码foobar登录localhost的baz数据库。

```
mongodb://fred:foobar@localhost/baz
```

连接 replica pair, 服务器1为example1.com服务器2为example2。

```
mongodb://example1.com:27017,example2.com:27017
```

连接 replica set 三台服务器 (端口 27017, 27018, 和27019):

```
mongodb://localhost,localhost:27018,localhost:27019
```

连接 replica set 三台服务器, 写入操作应用在主服务器 并且分布查询到从服务器。

```
mongodb://host1,host2,host3/?slaveOk=true
```

直接连接第一个服务器，无论是replica set一部分或者主服务器或者从服务器。

```
mongodb://host1,host2,host3/?connect=direct;slaveOk=true
```

当你的连接服务器有优先级，还需要列出所有服务器，你可以使用上述连接方式。

安全模式连接到localhost:

```
mongodb://localhost/?safe=true
```

以安全模式连接到replica set，并且等待至少两个复制服务器成功写入，超时时间设置为2秒。

```
mongodb://host1,host2,host3/?safe=true;w=2;wtimeoutMS=2000
```

### MongoDB--创建新的数据库

```json
use DATABASE_NAME		//创建新的数据库或者切换到已存在的制定数据库
db						//查询当前使用的数据库
show dbs				//产看所有数据库
//注意，新创建的数据库并不能在数据库的列表中显示，如果想要查看，需要在新数据库中插入一些数据


```

### MongoDB--删除数据库

```json
db.dropDatabase()		//删除数据库

```

### MongoDB--创建集合

```json
db.createCollection(name,options)		//创建一个集合

show tables				//展示集合
show collections		//展示集合

//创建带有参数的集合
> db.createCollection("mycol", 
                      { capped : true, autoIndexId : true, size : 6142800, max : 10000 } )


//注意：在MongoDB中，可以不需要创建集合，而在插入一些文档的时候，MongoDB会自动创建集合
> db.mycol2.insert({"name" : "菜鸟教程"})		//在集合mycol2中插入name字段，然后MongoDB会自动新建一个名为mycol2的集合
```

参数：

​	name:要创建的集合名称

​	options：可选参数，制定有关内存到校及索引的选项

##### options可选参数：

​	**capped**:  布尔值； 可选；为true则创建固定集合，固定集合有固定的大小的集合，当达到最大值时，会自动覆盖最早的文档。注意值为true时，必须制定size参数。

​	**autoIndexId:** 布尔值； 可选； 默认false，当值为true的时候会自动在_id字段创建索引。

​	**size:** 数值； 可选； 为固定集合指定一个最大值；以千字节计（KB）

​	**max:**  数值； 可选； 指定固定集合中包含文档的最大数量！



### mongoDB--删除集合

```json
db.collection.drop()	//删除集合返回true活着false

```

### MongoDB--插入文档

```json
db.COLLECTION_NAME.insert(document)			//通过insert（）活着save（）方法向集合中插入文档

//示例插入
>db.col.insert({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '菜鸟教程',
    url: 'http://www.runoob.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
})

//当当前的数据库没有col表的时候，MongoDB会自动创建新表并存储数据，否则会直接存储到已存在的表中

db.col.find()			//查看标col

//另一种插入方法：将数据定义为一个变量；然后执行插入操作
> document=({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '菜鸟教程',
    url: 'http://www.runoob.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
});		//定义变量

db.col.insert(document)		//变量插入
```

插入文档你也可以使用 db.col.save(document) 命令。如果不指定 _id 字段 save() 方法类似于 insert() 方法。如果指定 _id 字段，则会更新该 _id 的数据。

### MongoDB--更新文档

MongoDB 使用 **update()** 和 **save()** 方法来更新集合中的文档

```json
//updata()方法更新已经存在的文档
db.collection.update(
   <query>,	//检索条件
   <update>,		//更新对象和更新的操作符
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)
```

##### 参数说明：

- **query **: update的查询条件，类似sql update查询内where后面的。
- **update **: update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
- **upsert ** : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
- **multi ** : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
- **writeConcern ** :可选，抛出异常的级别。

```json
>db.col.insert({
    title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '菜鸟教程',
    url: 'http://www.runoob.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
})

>db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })   # 输出信息
> db.col.find().pretty()
{
        "_id" : ObjectId("56064f89ade2f21f36b03136"),
        "title" : "MongoDB",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "菜鸟教程",
        "url" : "http://www.runoob.com",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
```

### MongoDB--删除文档

```json
db.collection.remove(
   <query>,
   <justOne>
)
//注意，，同样可以参考上面的变量方法插入文档，使用变量添加删除条件
//如果你的 MongoDB 是 2.6 版本以后的，语法格式如下：

db.collection.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)
```

- **query **:（可选）删除的文档的条件。
- **justOne **: （可选）如果设为 true 或 1，则只删除一个文档，如果不设置该参数，或使用默认值 false，则删除所有匹配条件的文档。
- **writeConcern ** :（可选）抛出异常的级别。



### MongoDB--查询文档

```json
db.collection.find(query, projection)

db.col.find().pretty()		//以易读的方式读取数据
findOne（）
```

- **query** ：可选，使用查询操作符指定查询条件
- **projection** ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。

```json
//SQl的where
db.col.find({"likes":{$lt:50}}).pretty()	//小于
db.col.find({"likes":{$lte:50}}).pretty()	//小于或等于
db.col.find({"likes":{$gt:50}}).pretty()	//大于
db.col.find({"likes":{$gte:50}}).pretty()	//大于或等于
db.col.find({"likes":{$ne:50}}).pretty()	//不等于

//AND条件
>db.col.find({key1:value1, key2:value2}).pretty()

//MongoDB OR
>db.col.find(
   {
      $or: [
         {key1: value1}, {key2:value2}
      ]
   }
).pretty()
```



### MongoDB--条件操作符

- (>) 大于 - $gt
- (<) 小于 - $lt
- (>=) 大于等于 - $gte
- (<= ) 小于等于 - $lte

```json
db.col.find({likes : {$gt : 100}})		//获取 "col" 集合中 "likes" 大于 100 的数据
```

### MongoDB--$type操作符

$type操作符是基于BSON类型来检索集合中匹配的数据类型，并返回结果。

```json
//获取 "col" 集合中 title 为 String 的数据
db.col.find({"title" : {$type : 2}})
或
db.col.find({"title" : {$type : 'string'}})
```

### MongoDB--Limit与skip方法

下面语句，只会显示查询到的第二条语句：

```
>db.col.find({},{"title":1,_id:0}).limit(1).skip(1)
```



### MongoDB--sort方法排序

sort方法可以指定某参数排序，并使用1和-1来指定排序的方式（升序/降序）;下面语句指定likes参数降序排列；

```
>db.col.find({},{"title":1,_id:0}).sort({"likes":-1})
```

### MongoDB--索引

在 3.0.0 版本前创建索引方法为 db.collection.ensureIndex()，之后的版本使用了 db.collection.createIndex() 方法，ensureIndex() 还能用，但只是 createIndex() 的别名。

```json
>db.col.createIndex({"title":1})
//设置多个字段创建索引
>db.col.createIndex({"title":1,"description":-1})

```

注意：createIndex()可以接受可选参数的功能：

| Parameter          | Type          | Description                                                  |
| ------------------ | ------------- | ------------------------------------------------------------ |
| background         | Boolean       | 建索引过程会阻塞其它数据库操作，background可指定以后台方式创建索引，即增加 "background"    可选参数。  "background" 默认值为**false**。 |
| unique             | Boolean       | 建立的索引是否唯一。指定为true创建唯一索引。默认值为**false**. |
| name               | string        | 索引的名称。如果未指定，MongoDB的通过连接索引的字段名和排序顺序生成一个索引名称。 |
| dropDups           | Boolean       | 3.0+版本已废弃。在建立唯一索引时是否删除重复记录,指定 true 创建唯一索引。默认值为 **false**. |
| sparse             | Boolean       | 对文档中不存在的字段数据不启用索引；这个参数需要特别注意，如果设置为true的话，在索引字段中不会查询出不包含对应字段的文档.。默认值为 **false**. |
| expireAfterSeconds | integer       | 指定一个以秒为单位的数值，完成 TTL设定，设定集合的生存时间。 |
| v                  | index version | 索引的版本号。默认的索引版本取决于mongod创建索引时运行的版本。 |
| weights            | document      | 索引权重值，数值在 1 到 99,999 之间，表示该索引相对于其他索引字段的得分权重。 |
| default_language   | string        | 对于文本索引，该参数决定了停用词及词干和词器的规则的列表。 默认为英语 |
| language_override  | string        | 对于文本索引，该参数指定了包含在文档中的字段名，语言覆盖默认的language，默认值为 language. |



### MongoDB--聚合

MongoDB中聚合(aggregate)主要用于处理数据(诸如统计平均值,求和等)，并返回计算后的数据结果。有点类似sql语句中的 count(*)。

```json
{
   _id: ObjectId(7df78ad8902c)
   title: 'MongoDB Overview', 
   description: 'MongoDB is no sql database',
   by_user: 'runoob.com',
   url: 'http://www.runoob.com',
   tags: ['mongodb', 'database', 'NoSQL'],
   likes: 100
},
{
   _id: ObjectId(7df78ad8902d)
   title: 'NoSQL Overview', 
   description: 'No sql database is very fast',
   by_user: 'runoob.com',
   url: 'http://www.runoob.com',
   tags: ['mongodb', 'database', 'NoSQL'],
   likes: 10
},
{
   _id: ObjectId(7df78ad8902e)
   title: 'Neo4j Overview', 
   description: 'Neo4j is no sql database',
   by_user: 'Neo4j',
   url: 'http://www.neo4j.com',
   tags: ['neo4j', 'database', 'NoSQL'],
   likes: 750
},

> db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : 1}}}])
{
   "result" : [
      {
         "_id" : "runoob.com",
         "num_tutorial" : 2
      },
      {
         "_id" : "Neo4j",
         "num_tutorial" : 1
      }
   ],
   "ok" : 1
}
```

| 表达式    | 描述                                           | 实例                                                         |
| --------- | ---------------------------------------------- | ------------------------------------------------------------ |
| $sum      | 计算总和。                                     | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : "$likes"}}}]) |
| $avg      | 计算平均值                                     | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$avg : "$likes"}}}]) |
| $min      | 获取集合中所有文档对应值得最小值。             | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$min : "$likes"}}}]) |
| $max      | 获取集合中所有文档对应值得最大值。             | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$max : "$likes"}}}]) |
| $push     | 在结果文档中插入值到一个数组中。               | db.mycol.aggregate([{$group : {_id : "$by_user", url : {$push: "$url"}}}]) |
| $addToSet | 在结果文档中插入值到一个数组中，但不创建副本。 | db.mycol.aggregate([{$group : {_id : "$by_user", url : {$addToSet : "$url"}}}]) |
| $first    | 根据资源文档的排序获取第一个文档数据。         | db.mycol.aggregate([{$group : {_id : "$by_user", first_url : {$first : "$url"}}}]) |
| $last     | 根据资源文档的排序获取最后一个文档数据         | db.mycol.aggregate([{$group : {_id : "$by_user", last_url : {$last : "$url"}}}]) |

























































