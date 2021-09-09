# 来自UienZYC的python学习笔记 
# 列表和元组

_参考《树莓派Python编程入门与实战（第 2版）》_

_UienZYC_

***

## 元组

```python
# 创建元组
a=()
a=(1,2,3)
a=1,
a=1,2,3
# 引用
a[0]
a[-1]
a[0:2]  #注意[i,j)  i,j为索引值
a[0:6:2]  #步长2
```

```python
# 成员资格in
7 in a  #返回True
	if 7 in a: print("7 is in a")
	if 7 not in a: print("7 is not in a")
        
#获取元组中元素数量len()
len(a)  #返回一个int类型的数,用type(len(a))查看

#获得元组中最大最小的值min()&max()
>>> a=1,2,3.3
>>> min(a)
1
>>> max(a)
3.3
#字符串时为ASCII值比较
>>> a='abc','cba','bac'
>>> min(a)
'abc'
>>> max(a)
'cba'

#连接元组a+b
>>> a=1,2,3
>>> b='x','y','z'
>>> c=a+b
>>> print(c)
(1, 2, 3, 'x', 'y', 'z')
```

## 列表

```python
#创建列表
a=[]
a=[1,2,3]
#从可迭代对象创建
a=list(tuple1)
```

```python
#修改列表中某一个元素的值
a[1]='6'
#切片修改？有吗
#删除列表元素del a[0:3]  del a[0]
>>> a=[1,2,3,4,5,6]
>>> del a[0]
>>> a
[2, 3, 4, 5, 6]
>>> del a[0:3]
>>> a
[5, 6]

#弹出值（列表方法）list.pop() 缺省为最后一项
>>> a=[1,2,3,4]
>>> a.pop(0)
1
>>> a
[2, 3, 4]
>>> a.pop()
4
>>> a
[2, 3]

#添加新元素（列表方法）list.append(j) 将元素j加到末尾
>>> a=[1]
>>> a.append('jia')
>>> a
[1, 'jia']
#或者用list.insert(i,j) 将元素j添加到索引i之前
>>> a=[1,2,3]
>>> a.insert(0,'jia')
>>> a
['jia', 1, 2, 3]
使用append(),pop()实现数据结构栈

#原地连接列表（修改）list.extend()
>>> a=[1,2,3]
>>> b=['hello','world']
>>> a.extend(b)
>>> a
[1, 2, 3, 'hello', 'world']
```

```python
#统计某元素在列表中出现的次数 list.count(j)
>>> a=[1,2,2,3,3,3]
>>> a.count(3)
3
#就地排序（修改）list.sort()
>>> a=[3,2,1]
>>> a.sort()
>>> a
[1, 2, 3]
#就地倒序list.reverse()
#异地倒序（返回一个“可迭代的对象”）
>>> a=[1,6,7]
>>> b=reversed(a)
>>> type(b)
<class 'list_reverseiterator'>
>>> for x in b:
...     print(x)
...
7
6
1
#异地排序（返回一个新的列表）sorted(a)
>>> a=[3,2,1]
>>> b=sorted(a)
>>> b
[1, 2, 3]
#查找元素j第一次出现的索引 list.index(j) 返回索引
```

```python
#多维列表
>>> a=[[1,2,3],[4,5,6],[7,8,9]]
>>> a[0][0]
1
```

```python
# 遍历列表、元组
>>> a=[1,2,3]
>>> for x in a:
...     print(x)
...
1
2
3
```

```python
#列表解析（其中a是一个可迭代的对象）
>>> a=[1,2,3]
>>> b=[x**2 for x in a]
>>> print(b)
[1, 4, 9]
>>> a='hello','world'
>>> b=[x.upper() for x in a]
>>> b
['HELLO', 'WORLD']
```

```python
#range 类型
>>> a=range(0,4,1)
>>> for x in a:
...     print(x)
...
0
1
2
3
>>> print(a[3])
3
```


# 字典和集合

_UienZYC_

## 字典

```python
# 创建和引用和填充字典
>>> a={'name':'xuling','age':18}
>>> a['name']
'xuling'
# 逐个填充（这和修改列表中的某一元素很像，不同的是，字典可以借此创建一个新的元素）
>>> a['hobby']='karate'
>>> a
{'name': 'xuling', 'age': 18, 'hobby': 'karate'}
```

```python
# 获取字典中的值
# a.get(key,default)  法
>>> a.get('age','not found')
18
>>> a.get('gender','not found')
'not found'
# 这个方法的好处是如果没有对应的键就返回default的值，避免报错，如果没有指定default且没找到，则会返回none类型的一个none
>>> b=a.get('gender')
>>> print(b)
None
>>> type(b)
<class 'NoneType'>
```

```python
# 获取字典中的键（返回类型：字典键）
# a.key()
>>> b=a.keys()
>>> b
dict_keys(['name', 'age', 'hobby'])
# 接着可用for来遍历它
>>> for x in b:
...     print(a[x])
...
xuling
18
program
# 或者用列表解析来据此创建一个列表，并在有需要的情况下对其排序
>>> c=[x for x in b]
>>> c
['name', 'age', 'hobby']
# 实际上，因为sorted(a)返回一个列表，而这里的sorted的对象是一个可迭代的序列，这里并不需要先通过列表解析将字典键类型转化为列表类型，而可以直接使用sorted返回一个排序后的列表
>>> d=sorted(b)
>>> d
['age', 'hobby', 'name']
```

```python
# 删除一个字典元素，或者说删除一个键值对
# del dictionart_name[key]
>>> del a['hobby']
>>> a
{'name': 'xuling', 'age': 18}
# 如果这个键不存在就会报错，建议使用if来解决这个问题
>>> if 'name' in a:
...     print('you')
...
you
# 注意，这里检查成员key是否在dict中，检查value是没用的
```

```python
# 其他操作
>>> a
{'name': 'xuling', 'age': 18}
>>> len(a)
2
>>> a.keys()
dict_keys(['name', 'age'])
>>> a.values()
dict_values(['xuling', 18])
>>> a.items()
dict_items([('name', 'xuling'), ('age', 18)])
>>> b={'scores':90,'name':'zhangsan'}
>>> a.update(b)
>>> a
{'name': 'zhangsan', 'age': 18, 'scores': 90}
>>> a.clear()
>>> a
{}
```


