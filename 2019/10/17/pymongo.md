# python3操作MongoDB之PyMongo学习手册

### 连接MongoDB
```python
import pymongo
client = pymongo.MongoClient(host='localhost', port=27017)
# or
client = pymongo.MongoClient('mongodb://localhost:27017/')
```

### 指定数据库
```python
db = client.test
# or
db = client['test']
```

### 指定集合
```python
collection = db.students
# or
collection = db['students']
```

### 插入数据
```python
student = {
    'id': '20170101',
    'name': 'Jordan',
    'age': 20,
    'gender': 'male'
}
result = collection.insert(student)
result = collection.insert_one(student) # 推荐使用
print(result)
# 同时插入多个数据
student1 = {
    'id': '20170101',
    'name': 'Jordan',
    'age': 20,
    'gender': 'male'
}

student2 = {
    'id': '20170202',
    'name': 'Mike',
    'age': 21,
    'gender': 'male'
}

result = collection.insert([student1, student2])
result = collection.insert_many([student1, student2]) # 推荐使用
print(result)
```

### 查询数据
```python
# 查询单个数据
result = collection.find_one({'name': 'Mike'})
print(type(result))
print(result)
# 查询多个数据
results = collection.find({'age': 20})
print(results)
for result in results:
    print(result)
# 按条件查询
results = collection.find({'age': {'$gt': 20}})
# 按正则查询
results = collection.find({'name': {'$regex': '^M.*'}})

```
| 符号 | 含义 | 示例 |

| --- | ---- | --- |

| $lt | 小于 | {'age': {'$lt': 20}} |

| $gt | 大于 | {'age': {'$gt': 20}} |

| $lte | 小于等于 | {'age': {'$lte': 20}} |

| $gte | 大于等于 | {'age': {'$gte': 20}} |

| $ne | 不等于 | {'age': {'$ne': 20}} |

| $in | 在范围内 | {'age': {'$in': [20, 23]}} |

| $nin | 不在范围内 | {'age': {'$nin': [20, 23]}} |

### 计数
```python
# count
count = collection.find().count()
print(count)
# where & count
count = collection.find({'age': 20}).count()
print(count)
```

### 排序
```python
# 正序排列
results = collection.find().sort('name', pymongo.ASCENDING)
print([result['name'] for result in results])
```

### 分页查询
```python
# 正序排列
results = collection.find().sort('name', pymongo.ASCENDING).skip(2)
print([result['name'] for result in results])
```

### 更新
```python
# update
condition = {'name': 'Kevin'}
student = collection.find_one(condition)
student['age'] = 25
result = collection.update(condition, student)
print(result)
# $set
condition = {'name': 'Kevin'}
student = collection.find_one(condition)
student['age'] = 26
result = collection.update_one(condition, {'$set': student})
print(result)
```

### 删除
```python
result = collection.remove({'name': 'Kevin'})
print(result)
result = collection.delete_one({'name': 'Kevin'})
print(result)
result = collection.delete_many({'age': {'$lt': 25}})
print(result)
```

### 索引
```python
# 在students表的id字段创建索引
result = db.students.create_index([('id', pymongo.ASCENDING)], unique=True)
result = sorted(list(db.students.index_information()))
print(result)
```