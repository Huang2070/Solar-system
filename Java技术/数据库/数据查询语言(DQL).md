- [limit](#limit)
- [join](#join)
  - [join后on和where的区别](#join后on和where的区别)
- [case when](#case-when)
  - [语法](#语法)
  - [用法](#用法)
    - [已知数据按照另外一种方式进行分组](#已知数据按照另外一种方式进行分组)
    - [用一个SQL语句完成不同条件的分组](#用一个sql语句完成不同条件的分组)
- [where 和 having](#where-和-having)

# limit
select * from table limit m,n  
其中m是指记录开始的index，从0开始，表示第一条记录,
n是指从第m+1条开始，取n条。
select * from tablename limit 2,4  
即取出第3条至第6条，4条记录.

# join
![avatar](/sun/resource/join.jpg)
## join后on和where的区别
on比where起作用更早，先根据on条件进行多表的连接操作和筛选，生成一个临时表, 临时表的记录再通过where来筛选.

下面的例子来说明区别:  

建立两张简单的用来测试的表并添加数据，如下所示，一张表名为id_name，另一张表名为id_age.
id|name
-|-
1|a
2|b
3|c
4|d
5|e

id|age
-|-
1|19
2|22
3|21
4|22

<br>
<br>

**1.不添加条件**
```sql
select * from id_name t1 left join id_age t2
on t1.id = t2.id;
```
结果
id|name|id1|age
-|-|-|-
1|a|1|19
2|b|2|22
3|c|3|21
4|d|4|22
5|e|

<br>
<br>

**2.运行where条件**
```sql
select * from id_name t1 left join id_age t2
on t1.id = t2.id
where t2.age = 22;
```
结果
id|name|id1|age
-|-|-|-
2|b|2|22
4|d|4|22

说明where条件是在left join操作完成后所进行的条件筛选.

<br>
<br>

**3.运行on加条件**
```sql

select * from id_name t1 left join id_age t2
on t1.id = t2.id and t2.age = 22;
```
结果
id|name|id1|age
-|-|-|-
2|b|2|22
4|d|4|22
1|a
3|c
5|e

说明on条件是在left join之前先进行条件筛选，而后才对两个表格join操作.

<br>
<br>

# case when

## 语法
Case具有两种格式。简单Case函数和Case搜索函数。
```sql
--简单Case函数
CASE sex
WHEN '1' THEN '男'
WHEN '2' THEN '女'
ELSE '其他' END

--Case搜索函数
CASE 
WHEN sex = '1' THEN '男'
WHEN sex = '2' THEN '女'
ELSE '其他' END
```
这两种方式，可以实现相同的功能。简单Case函数的写法相对比较简洁，但是和Case搜索函数相比，功能方面会有些限制，比如写判断式(即条件语句)。  
还有一个需要注意的问题，Case函数只返回第一个符合条件的值，剩下的Case部分将会被自动忽略。
```sql
--比如说，下面这段SQL，你永远无法得到“第二类”这个结果
CASE 
WHEN col_1 IN ('a','b') THEN '第一类'
WHEN col_1 IN ('a') THEN '第二类'
ELSE '其他' END
```

## 用法
### 已知数据按照另外一种方式进行分组
```sql
# 简单case
SELECT  SUM(population),
CASE country
WHEN '中国'     THEN '亚洲'
WHEN '印度'     THEN '亚洲'
WHEN '日本'     THEN '亚洲'
WHEN '美国'     THEN '北美洲'
WHEN '加拿大'  THEN '北美洲'
WHEN '墨西哥'  THEN '北美洲'
ELSE '其他' END
FROM    Table_A
GROUP BY CASE country
WHEN '中国'     THEN '亚洲'
WHEN '印度'     THEN '亚洲'
WHEN '日本'     THEN '亚洲'
WHEN '美国'     THEN '北美洲'
WHEN '加拿大'  THEN '北美洲'
WHEN '墨西哥'  THEN '北美洲'
ELSE '其他' END;

# 搜索case
SELECT
CASE WHEN salary <= 500 THEN '1'
WHEN salary > 500 AND salary <= 600  THEN '2'
WHEN salary > 600 AND salary <= 800  THEN '3'
WHEN salary > 800 AND salary <= 1000 THEN '4'
ELSE NULL END salary_class,
COUNT(*)
FROM    Table_A
GROUP BY
CASE WHEN salary <= 500 THEN '1'
WHEN salary > 500 AND salary <= 600  THEN '2'
WHEN salary > 600 AND salary <= 800  THEN '3'
WHEN salary > 800 AND salary <= 1000 THEN '4'
ELSE NULL END;
```


### 用一个SQL语句完成不同条件的分组

国家（country）| 性别（sex）| 人口（population）
-|-|-
中国|1|340  
中国|2|260  
美国|1|45
美国|2|55
加拿大|1|51
加拿大|2|49
英国|1|40
英国|2|60

有上面数据
按照国家和性别进行分组, 结果如下:
国家|男|女
-|-|-
中国|340|260
美国|45|55
加拿大|51|49
英国|40|60

普通情况下，用UNION也可以实现用一条语句进行查询。但是那样增加消耗(两个Select部分)，而且SQL语句会比较长。  
下面是一个是用Case函数来完成这个功能的例子.
```sql
# 这样我们使用Select，完成对二维表的输出形式，充分显示了Case函数的强大。
SELECT country,
SUM( CASE WHEN sex = '1' THEN
population ELSE 0 END) as man,  --男性人口
SUM( CASE WHEN sex = '2' THEN
population ELSE 0 END) as woman   --女性人口
FROM  Table_A
GROUP BY country;
```


# where 和 having

>“Where” 是一个约束声明，使用Where来约束来之数据库的数据，Where是在结果返回之前起作用的，且Where中不能使用聚合函数。  
“Having”是一个过滤声明，是在查询返回结果集以后对查询结果进行的过滤操作，在Having中可以使用聚合函数。

SQL语言中一种特殊的函数。例如SUM, COUNT, MAX, AVG等。这些函数和其它函数的根本区别就是它们一般作用在多条记录上。

如：
SELECT SUM(population) FROM vv_t_bbc ;
 
这里的SUM作用在所有返回记录的population字段上，结果就是该查询只返回一个结果，即所有国家的总人口数。

而通过使用GROUP BY 子句，可以让SUM 和 COUNT 这些函数对属于一组的数据起作用。当你指定 GROUP BY region 时，只有属于同一个region（地区）的一组数据才将返回一行值，也就是说，表中所有除region（地区）外的字段，只能通过 SUM, COUNT等聚合函数运算后返回一个值。

下面再说说“HAVING”和“WHERE”：HAVING子句可以让我们筛选成组后的各组数据，WHERE子句在聚合前先筛选记录．也就是说作用在GROUP BY 子句和HAVING子句前；而 HAVING子句在聚合后对组记录进行筛选。

```sql
#显示每个地区的总人口数和总面积
SELECT region, SUM(population), SUM(area)
FROM bbc
GROUP BY region

#显示每个地区的总人口数和总面积．仅显示那些人口数量超过1000000的地区。
SELECT region, SUM(population), SUM(area)
FROM bbc
GROUP BY region
HAVING SUM(population)>1000000

#在这里，我们不能用where来筛选超过1000000的地区，因为表中不存在这样一条记录。相反，HAVING子句可以让我们筛选成组后的各组数据．
```

用having就一定要和group by连用，
用group by不一有having（它只是一个筛选条件用的）

只要条件里面的字段, 不是表里面原先有的字段就需要用having。


