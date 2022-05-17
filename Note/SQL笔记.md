### SQL笔记

#### 1、select from

> select为用户查询所需要的字段
>
> from为目标地址，即从哪查询

```sql
select name as 国家,continent as 大洲,population as 人口 
from world
# 上述查询可以略写为
select name 国家,continent 大洲,population 人口 
from world
```

