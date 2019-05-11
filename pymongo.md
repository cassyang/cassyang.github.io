### pymongo学习

#### 创建一个数据库
```python
#!/usr/bin/python3

import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["dbname"]
```
在monogodb中，只有在数据库中插入内容后，才会创建给客户端。即需要创建Collection然后插入一个document，数据库才会真正创建。

#### 判断数据库是否存在
```python
#!/usr/bin/python3

import pymongo

dblist = myclient.list_database_names()
# dblist = myclient.database_names() 在Python3.7+时候废弃该用法。
```

#### 判断Collection是否存在

```python
#!/usr/bin/python3

import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["dbname"]

collist = mydb.list_collection_names()
# collist = mydb.collection_names() 在Python3.7+时候废弃该用法。

if "testcollection" not in collist:
	mycol = mydb["testcollection"]
else:
	print("Collection is Exist!")
```

插入数据
---
- 集合中插入文档使用insert_one()方法，参数是dict(key, value).
```python
#!/usr/bin/python3

import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["mydb"]
mycol = mydb["mycol"]

mydict = {"name": "test", "score": 100}

x = mycol.insert_one(mydict)
# x是一个InsertOneResult对象，包含inserted_id属性，他是插入文档的id值
print(x.inserted_id) # 即对象的_id,如果插入document时没有指定_id，MongoDB会为每个文档添加一个唯一的id
```
- 插入多个文档使用insert_many()方法，该方法的第一参数是[{...}, {...}]
```python
x = mycol.insert_many(myList)
# 输出插入的所有文档对应的 _id 值
print(x.inserted_ids)
```
- 显示的声明_id
{"_id" : 1, "name" : balabala, ...}只需在dict中定义"_id"即可。

查询数据
---
- 查询一条数据find_one()方法，可以查询collection中的第一个数据。
- find()方法可以查询collentcion中的所有数据，类似SELECT *
- 指定字段查询，将需要返回的值设为1.
	``mycol.find({}, {"_id": 0, "name": 1, "class": 1})``
	这样查询返回结果会返回name和class而不返回_id.
	* 特别的只有_id之外，不能在同一个对象中同时指定0和1，如果设置了一个字段为0，则其他都为1。
- 指定条件查询，在find中设置参数来过滤。
	``mycol.find({"name" : "jhyang"})``
- 高级查询例如查询大于"H"的值，{"$gt" : "H"}
- 也可使用正则表达式来查询，例如{"$regex" : "^H"}
- 限制返回条目数使用limit()方法。
	``mycol.find().limit(3)``

修改数据
---
- 使用update_one()可以修改一条文件记录，第一个参数是查询条件，第二个参数是要修改的内容。
```python
myquery = {"name" : 1}
newvalues = {"name" : 10}
mycol.update_one(myquery, newvalues)
```
如果有多个条件满足的话，则只更新第一条。

- update_many()可以修改全匹配的记录。参数与update_one()一样

数据排序
---
- sort()方法可以指定升序/降序。第一个参数为要排序的字段，第二个参数指定排序规则，1为升序，-1为降序。默认为升序。
    ``mycol.find().sort("name", -1)``

删除数据
---
- delete_one()可以删除一个文档，第一个参数为查询对象，指定要删除哪些参数。
- delete_many()可以删除多个文档，参数为查询对象，与delete_one()类似。
- delete_many({}) 则会删除集合中的所有文档。
- drop() 可以删除整个collection
- remove(<query>, <justOne>=0)也可用于删除指定查询的数据，如果justOne写1，那么只删除1条，否则都删了。
```python
a = mycol.delete_one({"name" : 1})
b = mycol.delete_many({"name" : {"$regex" : "^F"}})
x = mycol.delete_many({})
print(x.deleted_count) # 输出删除文档是数量
mycol.drop() # 成功返回true，如果删除失败(集合不存在)返回false
```
