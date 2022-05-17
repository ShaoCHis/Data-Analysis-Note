### SQL笔记

[toc]

[练习网站](https://sqlzoo.net/wiki/SELECT_basics)

#### 1、select from

> select为用户查询所需要的字段
>
> from为目标地址，即从哪查询
>
> 分号用于区分多段查询

```sql
select name as 国家,continent as 大洲,population as 人口 
from world;
# 上述查询可以略写为-----》as可以用作字段别名，as可省略
select name 国家,continent 大洲,population 人口 
from world;
```

```sql
# 查询表中所有的字段，并以表中的顺序进行显示
select *
from world
```

```sql
# select中去重---->distinct对重复数据进行去重操作
select distinct continent
from world
# select distinct continent,name,population
# 即distinct后续跟多个字段，将多个字段的数据看作一个元组(continent,name,population)，若三个数据均相同才看作一个重复数据进行去重操作，否则便是不同的数据
# 同时distinct关键字仅能跟在select字段后，不能放置于中间
```

```sql
# 计算字段，在select子句中可以添加自定义的计算字段
select name 国家,continent 大洲,gdp,gdp/population 人均gdp
from world
```

#### 2、where

>select
>
>from
>
>where
>
>where即select from的**判断条件子句**，即限定查询行必须满足的条件

运算符一览：

| 运算符           | 描述                             |
| ---------------- | -------------------------------- |
| =                | 等于                             |
| >                | 大于                             |
| <                | 小于                             |
| >=               | 大于等于                         |
| <=               | 小于等于                         |
| <>或!=           | 不等于                           |
| between...and... | 在两个指定值之间，包含这两个值   |
| in               | 条件范围筛选                     |
| not in           | 不在该条件范围                   |
| is null          | 为空值                           |
| is not null      | 不为空值                         |
| and              | 逻辑运算符：与                   |
| or               | 逻辑运算符：或                   |
| not              | 逻辑运算符：非（一般与其他连用） |

- =,>,<,!=,<>为比较运算符，用于判断表中的哪些数据符合条件
- and、or和not为逻辑运算符
- between and 用于查询两个值之间范围的值（包含这两个值）
- in，用于查询指定条件范围内的数据，一般为in（xxxx，xxxx，......），用括号将条件括起来
- is null，用于查询空值（NULL），空值不同于0，也不同于null字符串

```sql
# 查询人口至少为2亿的国家和人均gdp
select name,gdp/population 人均gdp
from world
where population>=200000000
```

```sql
# 查询德国的人口
select population
from world
where name='Germany'			# 注意使用单引号表示字符串
```

```sql
# 查询瑞典、丹麦和挪威的国家名和人口
select name,population
from world
where name in('Sweden','Norway','Denmark') 			# 在in语句判断中，同样需要使用单引号表示字符串
```

```sql
# 查询面积在250000和300000之间的国家名和面积大小
select name,area
from world
where area between 250000 and 300000
```

```sql
# 模糊查询like '通配符+字符'
# 通配符用来匹配值的一部分，跟在like后面进行数据过滤常用的通配符有%和_，%可以用来匹配多个字符，可以是零个、一个也可以是多个字符，_仅能用来匹配单个字符

# 查询国家名以c开头以ia结尾的国家
select name
from world
where name like 'c%ia'

# 查询国家名中第二个字符为't'的国家
select name
from world
where name like '_t%'

# 查询国家名中含有两个o且被两个字符隔开的国家名
select name
from world
where name like '%o__o%'
```

##### 多条件查询

> 使用and或者or逻辑运算符对多个条件进行组合筛选想要的数据

```sql
# 查询国家名中含有三个a且面积大于60万的国家及面积或者人口大于13亿且面积大于500万的国家及其面积
select name,area
from world
where (name like '%a%a%a%' and area>=600000) or (population>=1300000000 and area >5000000)
```

```sql
# 查询南美洲所有国家的名称以及它们以百万（1000000）为单位的人口数量
select name,population/1000000 人口数量（百万）
from world
where continent='South america'
```

```sql
# 查询1980年除诺贝尔化学奖和诺贝尔医学奖外其余奖项获奖者的所有信息
select *
from nobel
where yr=1980 and subject not in ('Chemistry','medicine')
```

```sql
# 查询既包含有元音字母（a,e,i,o,u)，同时国家名中没有空格的国家，最后显示他们的名字
select name
from world
where name like '%a%' and name like '%e%' and name like '%i%' and name like '%o%' and name like '%u%' and name not like '% %'
```

```sql
# 查询1910前以前（不含1910）诺贝尔医学奖获得者和2004年以后诺贝尔文学奖获得者的所有信息
select *
from nobel
where (yr<1910 and subject='medicine') or (yr>=2004 and subject='literature')
```

#### 3、order by

> select
>
> from
>
> where
>
> order by 字段名 asc|desc
>
> order by 字段名 asc|desc	规定查询出的结果集显示的顺序；默认为asc升序

```sql
# 查询姓名以Sir开头的获奖者，获奖年份和科目，查询结果按照年份从近到远排序，再按姓名顺序升序排序
select*
from nobel
where winner like 'Sir%'
order by yr desc,winner asc
```

```sql
# 查询1984年所有获奖者的姓名和奖项科目。结果将诺贝尔化学奖和物理学奖排在最后，然后按照科目排序，再按照获奖者姓名排序
select*
from nobel
where yr=1984
order by subject in ('chemistry','physics'),subject,winner
```

#### 4、limit

> select
>
> from
>
> where
>
> order by
>
> limit  [位置偏移量，]行数	限制结果显示的行数；limit子句是可选项，行数是子句中的必选参数，参数位置偏移量是可选参数

```sql
# 查询面积排名前三的国家
select name
from world
order by area desc
limit 3				# limit n 返回前n条数据
```

```sql
# 查询人口数第4到第7的国家和人口
select name,population
from world
order by population desc
limit 3,4			# limit n,m 偏移3，即从第4个位置开始取值，取4条
# limit x,n 		查询结果返回第x+1行开始的n行到x+n行
```

```sql
# 查询nobel表中第100行到120行的数据
select*
from nobel
limit 99,20
```

#### 5、聚合函数&group by

> select
>
> from
>
> where
>
> group by 字段名
>
> order by 字段名 asc|desc
>
> limit [位置偏移，]行数
>
> group by 字段名 规定依据哪个字段分组聚合，使用该子句是为了依据相同字段值分组后进行聚合运算，常和聚合函数联用
>
> 聚合函数适用于需要获取数据的汇总信息，例如某字段行数、某字段平均值、某字段中最大最小数等

单独使用聚合函数：

```sql
# 查询非洲总人数
select sum(population)				# sum求和，max最大值，min最小值，avg平均值
from world
where continent='Africa'
```

```sql
# 计算表格函数
select count(*)
from world
```

```sql
```





