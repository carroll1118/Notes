
Mongo中的一些概念：

| SQL术语/概念  |  MongoDB术语/概念 | 解释|
------------------ | ------------------ | -------------
| database | database|数据库
| table	|collection|	数据库表/集合
row|	document	|数据记录行/文档
column |	field	|数据字段/域
index|	index |	索引
table joins	| 	|表连接,MongoDB不支持
primary key|	primary key	|主键,MongoDB自动将_id字段设置为主键

```sql
"show dbs" 命令可以显示所有数据的列表。
"db" 命令可以显示当前数据库对象或集合。
"use"命令，可以连接到一个指定的数据库。
```
* 创建数据库的语法格式
```sql
use DATABASE_NAME

如果数据库不存在，则创建数据库，否则切换到指定数据库
```
* 删除数据库的语法格式

```sql
db.dropDatabase()

删除当前数据库，默认为 test，你可以使用 db 命令查看当前数据库名。
```
* createCollection() 方法来创建集合,语法格式
```sql
db.createCollection(name, options)
参数说明：
name: 要创建的集合名称
options: 可选参数, 指定有关内存大小及索引的选项

//MongoDB 中，你不需要创建集合。当你插入一些文档时，MongoDB 会自动创建集合。
```

* 集合删除语法格式
```sql
db.collection.drop()  
```
* 使用 insert() 或 save() 方法向集合中插入文档，语法格式
```sql
 db.COLLECTION_NAME.insert(document)  
```
* update() 方法用于更新已存在的文档。语法格式

```sql
db.collection.update(    
	<query>, 
	<update>, 
	{       
		upsert: <boolean>,   
		multi: <boolean>,  
		writeConcern: <document>
	}
)
参数说明：
query : update的查询条件，类似sql update查询内where后面的。
update : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
upsert : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
multi : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
writeConcern :可选，抛出异常的级别。
```
* save() 方法通过传入的文档来替换已有文档。语法格式

```sql
db.collection.save(    
	<document>,     
	{      
		writeConcern: <document> 
	}  
)  
参数说明：
document : 文档数据。
writeConcern :可选，抛出异常的级别。
```
* remove() 方法的基本语法格式
```sql
db.collection.remove(     
	<query>,     
	{       
		justOne: <boolean>,
		writeConcern: <document> 
	} 
)
参数说明：
query :（可选）删除的文档的条件。
justOne : （可选）如果设为 true 或 1，则只删除一个文档。
writeConcern :（可选）抛出异常的级别。
```
* 查询数据的语法格式如下：
```sql
db.COLLECTION_NAME.find()
find() 方法以非结构化的方式来显示所有文档。
```
* 以易读的方式来读取数据，可以使用 pretty() 方法，语法格式
```sql
db.col.find().pretty()  
pretty() 方法以格式化的方式来显示所有文档。
```
* MongoDB 与 RDBMS Where 语句比较

如果你熟悉常规的 SQL 数据，通过下表可以更好的理解 MongoDB 的条件语句查询：

| 操作	| 格式	 | 范例|	RDBMS中的类似语句|
|-----|-------|------|-----|
等于|	{<key>:<value>}	|db.col.find({"by":"菜鸟教程"}).pretty()	|where by = '菜鸟教程'
小于|	{<key>:{$lt:<value>}}|	db.col.find({"likes":{$lt:50}}).pretty()|	where likes < 50
小于或等于|	{<key>:{$lte:<value>}}	|db.col.find({"likes":{$lte:50}}).pretty()|	where likes <= 50
大于	|{<key>:{$gt:<value>}}	|db.col.find({"likes":{$gt:50}}).pretty()|	where likes > 50
大于或等于|	{<key>:{$gte:<value>}}	|db.col.find({"likes":{$gte:50}}).pretty()	|where likes >= 50
不等于|	{<key>:{$ne:<value>}}	|db.col.find({"likes":{$ne:50}}).pretty()	|where likes != 50

* find() 方法可以传入多个键(key)，每个键(key)以逗号隔开，语法格式
```sql
db.col.find({key1:value1, key2:value2}).pretty()  
```
* OR 条件语句使用了关键字 $or,语法格式

```sql
db.col.find(  {   $or: [  {key1: value1}, {key2:value2} ] } ).pretty()  
```
* MongoDB中条件操作符
```sql
(>) 大于 - $gt
(<) 小于 - $lt
(>=) 大于等于 - $gte
(<= ) 小于等于 - $lte
```
* limit()方法基本语法格式
```sql
db.COLLECTION_NAME.find().limit(NUMBER)  
```
* skip()方法来跳过指定数量的数据，skip() 方法脚本语法格式

```sql
db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)  
```
* sort()方法基本语法如下所示：

```sql
//sort()方法对数据进行排序，sort()方法可以通过参数指定排序的字段，并使用 1 和 -1 来指定排序的方式，
// 其中 1 为升序排列，而-1是用于降序排列。
db.COLLECTION_NAME.find().sort({KEY:1})
```

