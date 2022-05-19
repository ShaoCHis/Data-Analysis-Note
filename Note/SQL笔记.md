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
# subject in ('chemistry','physics')这种条件判断语句会将在in中的属性看作1，其他subject的属性为0，然后默认为asc升序排序，即1会排在0的后面
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
# 尝试运行以下sql语句
select count(name),count(continent),count(area),count(population),count(gdp),count(*)
from world

select sum(gdp)/195,sum(gdp)/189,avg(gdp)
from world
# 可以发现sum(gdp)/189的值与avg(gdp)相等，即聚合函数不会计入空值,忽略空值
```

单独使用group by：

```sql
# 以大洲分组，查询人口
select sum(population),continent
from world
[where continent='Africa']	# 非洲人口总人数
group by continent
# 没有出现在group by中的属性必须出现在聚合函数中
```

```sql
# 查询每个大洲和每个大洲的国家数量
select count(name),continent
from world
group by continent
```

```sql
# 查询2013至2015年每年每个科目的获奖人数，结果按年份从大到小，人数从大到小排序
select yr,subject,count(winner) counts
from nobel
where yr between 2013 and 2015
group by yr,subject
order by yr desc,counts desc
# select为最后运行的代码行
```

聚合函数：

| 函数function | 说明             |
| ------------ | ---------------- |
| AVG()        | 返回某列的均值   |
| COUNT()      | 返回某列的行数   |
| MAX()        | 返回某列的最大值 |
| MIN()        | 返回某列的最小值 |
| SUM()        | 返回某列的和     |

标准语法：

- select 字段名1,聚合函数(字段名)
- from 表名
- [where 表达式]
- [group by 字段名1]
- [order by 字段名 asc|desc]
- [limit [位置偏移量,]行数]

```sql
# 计算Estonia, Latvia, Lithuania这几个国家的总人口数
select sum(population)
from world
where name in ('Estonia','Latvia','Lithuania')
```

```sql
# 查询每个大洲和该大洲里人口数超过1千万的国家的数量
select count(name),continent
from world
where population>10000000
group by continent
```

#### 6、having&简单运行原理

> - select 字段名
> - from 表名
> - [where 表达式]
> - [group by 字段名]
> - [having 表达式]
> - [order by 字段名 asc|desc]
> - [limit [位置偏移量,]行数]
>
> having 表达式限定分组聚合后的查询行必须满足的条件；使用该子句是为了对group by分组后的数据进行筛选
>
> having等价于对group by的where语句

```sql
# 查询总人口数量至少为1亿的大洲
select continent
from world
group by continent
having sum(population)>100000000
# 同时having表达式中可以使用聚合函数，where表达式中不可以，因为where是对原表的行数据筛选，having是group by之后的数据进行筛选
# 建议对行数据进行筛选使用where子句，对含有聚合函数的筛选使用having子句
# having在聚合后进行判断，where在聚合前进行判断
```

```sql
# 查询总人口数至少为3亿的大洲和其平均gdp，其中只有gdp高于200亿且人口数大于6000万或者gdp低于80亿且首都中含有三个a的国家的计入计算，最后按国家数从大到小排序，只显示第一行
select continent,avg(gdp)
from world
where (gdp>20000000000 and population>60000000) or (gdp<8000000000 and capital like '%a%a%a%')
group by continent
having sum(population)>300000000
order by count(name) desc
limit 1
```

> 运行原理：from-where-group by-having -order by-limit-select
>
> - 执行from语句从数据库中调取复制一份表格
> - 执行where语句在复制的表格中筛选出符合条件的数据行
> - 执行group by语句依据指定的字段对筛选后的数据分区，将依据的字段进行去重分组，相当于Excel建立了一个数据透视表，添加了行标签
> - 执行having语句筛选满足条件的分组
> - 执行order by语句对满足条件的分组进行排序
> - 执行limit语句对排序后的数据进行显示的行数限制
> - 执行select语句，提取最后要显示的字段

```sql
# 查询人均gdp大于3000的大洲及其人口数，仅gdp在200亿和300亿之间的国家计入计算
select continent,sum(population)
from world
where gdp between 20000000000 and 30000000000
group by continent
having sum(gdp)/sum(population)>3000
```

#### 7、部分常见函数

- 数学函数

  > round(x,y)	-四舍五入函数
  >
  > > round函数对x值进行四舍五入，精确到小数点后y位
  > >
  > > y为负值时，保留小数点左边相应的位数为0，不进行四舍五入
  > >
  > > ```
  > > 例如：round(3.15,1)返回3.2；round(14.15,-1)返回10
  > > ```

- 字符串函数

  > concat(s1,s2,...)	-连接字符串函数
  >
  > > concat函数返回连接参数s1、s2等产生的字符串
  > >
  > > 任意参数为null时，则返回null
  > >
  > > ```
  > > 例如：concat('My','SQL')返回MySQL;concat('My',null,'SQL')返回null
  > > ```
  >
  > replace(s,s1,s2)	-替换函数
  >
  > > replace函数使用字符串s2代替s中所有的s1
  > >
  > > ```
  > > 例如：replace('MYSQLMYSQL','SQL','sql')返回MYsqlMYsql
  > > ```
  >
  > left(s,n)、right(s,n)&substring(s,n,len)	-截取字符串一部分的函数
  >
  > >left函数返回字符串s最左边n个字符
  > >
  > >right函数返回字符串最右边n个字符
  > >
  > >substring函数返回字符串s从第n个字符起取长度为len的子字符串，n也可以为负值，则从**倒数**第n个字符起取长度为len的子字符串，没有len值则取从第n个字符起到最后一位
  > >
  > >```
  > >例如：left('abcdefg',3)返回abc，right('abcdefg',3)返回efg，substring('abcdefg',2,3)返回bcd，substring('abcdrfg',-2,3)返回fg，substring('abcdefg',2)返回bcdefg
  > >```

- 数据类型转换函数

  >cast(x as type)	-转换数据类型函数
  >
  >> cast函数将一个类型的x值转换为另一个类型的值
  >>
  >> type参数可以填写char(n)、date、time、datetime、decimal等转换为对应的数据类型

- 日期时间函数

  > year(date)、month(date)&day(date)	-获取年月日的函数
  >
  > >date可以是年月日组成的日期，也可以是年月日时分秒组成的日期时间
  > >
  > >year(date)返回日期格式中的年份
  > >
  > >month(date)返回日期格式中的月份
  > >
  > >day(date)返回年日期格式中的月份
  > >
  > >```
  > >例如：year('2021-08-03')返回2021，month('2021-08-03')返回8，day('2021-08-03')返回3
  > >```
  >
  > date_add(date,interval expr type)&date_sub(date,interval expr type)	-对指定起始时间进行加减操作
  >
  > > date用来指定起始时间
  > >
  > > date可以是年月日组成的日期，也可以是年月日时分秒组成的日期时间
  > >
  > > expr用来指定从起始时间添加或减去的时间间隔
  > >
  > > type指示expr被解释的方式，type可以是以下值
  > >
  > > ```
  > > 主要使用：second、minute、hour、day、week、month、quarter、year
  > > ```
  > >
  > > date_add函数对起始时间进行加操作，date_sub函数对起始时间进行减操作
  > >
  > > ```
  > > 例如：date_add('2021-08-03 23:59:59',interval 1 second)返回2021-08-04 24:00:00,date_sub('2021-08-03 23:59:59',interval 2 month)返回2021-06-03 23:59:59
  > > ```
  >
  > datediff(date1,date2)	-计算两个日期之间间隔的天数
  >
  > > datediff函数由date1-date2计算出间隔的时间，只有date的**日期部分参与计算，时间不参与**
  > >
  > > ```
  > > 例如：datediff('2021-06-08','2021-06-01')返回7，datediff('2021-06-08 23:59:59','2021-06-01 21:00:00')返回7，datediff('2021-06-01','2021-06-08')返回-7
  > > ```
  >
  > date_format(date,format)	-将日期和时间格式化
  >
  > >date_format函数根据format指定的格式显示date值
  > >
  > >```
  > >例如：date_format('2018-06-01 16:23:12','%b%d%Y%h:%i%p')返回Jun 01 2018 04:23 PM，date_format('2018-06-01 16:23:12','%Y%d%m')返回2018/01/06
  > >```

- 条件判断函数

  > if(expr,v1,v2)
  >
  > > 如果表达式expr是true返回值v1，否则返回v2
  > >
  > > ```
  > > 例如：if(1<2,'Y','N')返回Y，if(1>2,'Y','N')返回N
  > > ```
  >
  > case when
  >
  > > case expr when v1 then r1 [when v2 then r2]....[else rn]	end
  > >
  > > ```
  > > 例如：case 2 when 1 then 'one' when 2 then 'two' else 'more' end	返回two
  > > case when 1<0 then 'T' else 'F' end 返回F
  > > ```

```sql
# 将0、1与>1替换为文本信息
select recovered 累计人数,case when recovered=1 then 'one' when recovered>1 then 'more' else '0' end
from covid
where recovered>0
# 将数字1替换为one,>1替换为more
```

```sql
# year、month、day函数
select whn 更新时间,year(whn) 年,month(whn) 月,day(whn) 日
from covid
where recovered>0
```

```sql
# date_add函数
select whn 更新时间,date_add(whn,interval 2 day) 加两天
from covid
where recovered>0
```

```sql
# replace函数
select distinct name,replace(name,'a','替换') 替换
from covid
```

```sql
# round函数和concat函数
select deaths,recovered,confirmed,concat(round((recovered/confirmed)*100,2),'%')
from covid
where recovered/confirmed>0.3
```

```sql
# 查询国家名称及其首都名称都以相同的字母开头的国家名及其首都，且不能包括国家名称和首都名称完全相同的情况
select name,capital
from world
where left(name,1)=left(capital,1) and name!=capital
```

```sql
# 查询首都和名称，其中首都需是国家名称的扩展
select name,capital
from world
where capital like concat('%',name,'%') and capital!=name
```

#### 8、窗口函数

窗口函数，也叫OLAP函数(Online Anallytical Processing，联机分析处理)，可以对数据库数据进行实时分析处理

窗口函数的基本语法如下：

```
<窗口函数> over(partition by <用于分组的列名>
			  order by <用于排序的列名>)
```

<窗口函数>的位置，可以放以下两种函数：

1）专用窗口函数，包括后面要讲到的rank、dense_rank、row_number等专用窗口函数

2）聚合函数，如sum、avg、count、max、min等

因为窗口函数是对where或者group by子句处理后的结果进行操作，**所以窗口函数原则上只能写在select子句中**

使用例子：

![](https://pic2.zhimg.com/80/v2-f8c3b3deb99122d75bb506fdbea81c8d_720w.jpg)

```sql
# 每个班内按成绩排名
# select子句中，rank是排序的函数；这句话可以分为两部分
# 1）每个班级内，按班级分组		-partition by用来对表分组。在本例中，我们指定了按“班级”分组（partition by 班级）
# 2）按成绩排名		-order by子句的功能是对分组后的结果进行排序，默认是asc升序。
select *,
	rank() over (partition by 班级,
                order by 成绩 desc)	as ranking
from 班级表
```

![](https://pic3.zhimg.com/80/v2-3285d1d648de9f90864000d58847087a_720w.jpg)

**group by分组汇总后改变了表的行数，一行只有一个类别。而partiition by和rank函数不会减少原表中的行数**

简单来说，窗口函数具有以下功能：

- 同时具有分组和排序的功能

- 不减少原表的行数

- 语法如下

  ```sql
  <窗口函数>	over (partition by <用于分组的列名>
               order by <用于排序的列名>)
  ```

其他专业的窗口函数：dense_rank，row_number区别

```sql
select *，
	rank() over (order by 成绩 desc) as ranking,				# rank将进行序号的排序，值相同并列，但索引值会累加
	dense_rank() over (order by 成绩 desc) as dense_rank,		 # dense_rank同样排序，值相同并列，索引值不会累加
	row_number() over (order by 成绩 desc) as row_num,		 # row_number即进行每一行的编号操作
from 班级表
```

![](https://pic2.zhimg.com/80/v2-ad1d86f5a5b9f0ef684907b20b341099_720w.jpg)

**聚合函数用作窗口函数**

聚合窗口函数和上面提到的专用窗口函数用法完全相同，只需要把聚合函数写在窗口函数的位置即可，但是函数后面括号里面不能为空，需要指定聚合列名。

```sql
# 聚合函数sum在窗口函数中，是对自身记录、及位于自身记录以上的数据进行求和的结果
select *,
	sum(成绩) over (order by 学号) as current_sum,
	avg(成绩) over (order by 学号) as current_avg,
	count(成绩) over (order by 学号) as current_count,
	max(成绩) over (order by 学号) as current_max,
	min(成绩) over (order by 学号) as current_min,
from 班级表
```

![](https://pic2.zhimg.com/80/v2-c48f0218306f65049fcf9f98c184226d_720w.jpg)

