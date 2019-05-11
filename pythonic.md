## 让代码更加pythonic

输出字符串
---
```python
name = 'jhyang'
age  = 24
# not pythonic
print "Hi, I'm " + name + " and I'm " + str(age) + " years old."
# nearly, but not pythonic
print "Hi, I'm %s and I'm %d years old." % (name, age)
# pythonic
print "Hi, I'm {} and I'm years old.".format(name, age)

print "Hi, I'm {1} years old and my name is {0}, yeah {1} years.".format(name, age)

# if you have a dictionary then this is still better:
data = {'day': 'Sunday', 'office': 'Home', 'other': 'UNUSED'}
print "On {day} I was working in my {office}!".format(**data)
```

合并字典
---
```python
route = {'id': 271, 'title': 'Fast apps'}                                                                                     
query = {'id': 1, 'render_fast': True}
post  = {'email': 'jhyang@jhyang.com', 'name': 'jhyang'}

# non-pythonic
m1 = {}
for k in query:
    m1[k] = query[k]
for k in post:
    m1[k] = post[k]
for k in route:
    m1[k] = route[k]

print m1

# pythonic
m2 = dict(dict(query, **post), **route)
print m2
```

变量交换
---
```python
a = 1                                                                                                                         
b = 2 
# non-pythonic, like c/java
tmp = a 
a = b 
b = tmp 
print "a={}, b={}".format(a, b)
# pythonic
a, b = b, a
print "a={}, b={}".format(a, b)
```

推导式
---
```python
num_list1 = [num for num in xrange(1,6)]                                                                                      
print num_list1
num_list2 = [num for num in xrange(1, 6) if num%2 == 1]
print num_list2

student = [('name', 'Tom'), ('age', 18)]
student = {item[0]: item[1] for item in student}
print student
```
