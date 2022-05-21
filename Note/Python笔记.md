### Python笔记

> 以下python笔记为个人记录，不包含基础的用法与知识部分

[toc]

#### 1、Python函数

```python
# strip函数去除首尾的空格
b = '          hello        '
b.strip()     # hello
# 去除中间的空格
b = 'I am happy'
b.replace(' ','')	# Iamhappy
```

```python
L1 = ['hello','python','and','java']
enumerate(L1)	# (0,'hello'),(1,'python'),(2,'and'),(3,'java')返回列表中的索引值与其对应的value
# 通过索引，可以取出对应的值
list(enumerate(L1))
L1[1]	# 'python'
```

```python
# python利用切片取值
L1[0:1]	# 左闭右开，'hello'
# 在尾部添加新的元素
L1.append('C+++')	# L1 = ['hello','python','and','java','C++']
```

```python
# 字典的生成
# 生成方式1：直接码上键与值
EVA1 = {'凌波丽':'EVA零号机','点真丝':'EVA初号机','明日香':'EVA二号机'}
# 生成方式2：用zip函数匹配列表，再用dict生成字典
EVA2 = dict(zip(['凌波丽','点真丝','明日香'],['EVA零号机','EVA初号机','EVA二号机']))
# EVA2 = {'凌波丽':'EVA零号机','点真丝':'EVA初号机','明日香':'EVA二号机'}

# 可以用dict.keys()查看键值，dict.values()查看值
EVA1.keys()	# ['凌波丽','点真丝','明日香']
# 可以通过键去查看值
EVA1['凌波丽']	# EVA零号机
```

#### 2、numpy

> Numberical Python:数字化python，一个科学运算与人工智能领域最常见的模块

ndarray:N维数组对象，我们简单的将其理解为一个数据类型即可

```python
ar1 = np.array([1,2,3])
print(ar1,type(ar1))
# 注意这里转换为一维数组，长度为3
import numpy as np
ar1 = np.array([(1,2),(3,0)])
print(ar1,type(ar1))
# 这里转换为2维数组
```

```python
# 元素类型和转换
np.array(range(1,4)).dtype	# dtype('int32')
np.array(range(1,4)).astype('float')	# 将元素类型转化为浮点型，array([1.,2.,3.])
```

```python
ar = np.array(range(0,12))
ar.shape	#(12,)
# 将数组重塑维3行4列
new_ar = ar.reshape(3,4)
```

```python
# 针对二维数组，axis=0代表自上而下的，axis=1代表从左到右的
new_ar.sum(axis = 0)	# (12,15,18,21)
new_ar.sum(axis = 1)	# (6,22,38)
```

```python
# 逻辑运算
arr == arr4	# 会将每一个值进行一一比较，如果值相同，返回True，不同返回False
```

```python
# 索引和切片
arr = np.array([5,5,3,4,1])
print(arr[2:4])	# 3,4
# 二维数组切片
arr2d = np.array([[1,2,3],[4,5,6],[7,8,9]])
print(arr2d[2])	# 7,8,9
print(arr2d[2][2])	# 9

print(arr2d[:2])		# [1,2,3],[4,5,6]
print(arr2d[:2][1:])	# 基于第一次结果取值	[4,5,6]
print(arr2d[:2,1:])		# 在第一次结果内部取值	[2,3][5,6]
```

```python
arr = np.empty((4,4))
for i in range(4):
    arr[i] = (i+1)**2	[1,1,1,1][4,4,4,4][9,9,9,9][16,16,16,16]
arr[[2,3,0]]	# 选取索引为2，3，0对应的行	[9,9,9,9][16,16,16,16][1,1,1,1]
arr[[0,2,3]]	# 选取索引为0，2，3对应的行	[1,1,1,1][9,9,9,9][16,16,16,16]
arr[[0,2,3]][[0,2,1]]	# 选取索引为0，2，3对应的行，再选取索引为0，2，1对应的行[1,1,1,1][16,16,16,16][9,9,9,9]
```

```python
# 三元表达式where
xarr = np.array([1.1,1.2,1.3,1.4,1.5])
yarr = np.array([2.1,2.2,2.3,2.4,2.5])
result = np.where(xarr > 1.3, xarr, yarr)
result	# [2.1,2.2,2.3,1.4,1.5]
```

```python
tempre = np.array([28,31,30,25,26])
result = np.where(tempre > 28,'地下室','室外')
result
```

```python
arr.sum()	# 求和
arr.mean()	# 求平均
arr.sort(axis = index)	# 排序
```

#### 3、pandas

> pandas中有两种数据类型，分别为Series和DataFrame

##### 3.1、数据类型Series

> Series由values和index组成，从另一个角度理解，可以认为Series是一个长度固定且有序的字典

```python
import pandas as pd
eva = {'凌波丽':0,'点真丝':1}	# index:values
eva_s = pd.Series(eva)
eva_s.values
eva_s.index
```

```python
# Series可以通过切片进行取值
eva_s[0]	# 取出的是value
eva_s[0:1]
```

```python
# 使用布尔索引
eva_s == 0	# 凌波丽:True;点真丝:False
eva_s[eva_s==0]	# 凌波丽:0
```

##### 3.2、DataFrame

> 本节采用pandas与DataFrame共同对标SQL的方式进行讲解

