### 获取分组后取某字段最大一条记录
```sql
#按type分组,取typeindex最大值
select * from test as a 
where typeindex = 
(
    select max(b.typeindex) 
    from test as b 
    where a.type = b.type 
);
```

### 合并多条sql查询记录到同一行
```sql
--如果每条sql的查询结果都是一个字段,可以用逗号合并查询结果到一行
select
(SELECT 字段1 FROM 表1 where id = 1) as t1,
(SELECT 字段2 FROM 表2 where id = 1) as t2,
(SELECT 字段3 FROM 表3 where id = 1) as t3

--如果每条sql查询结果有多个, 则写法如下
select * from
(SELECT 字段1,字段2,字段3  FROM 表1 where id = 1) as t1,
(SELECT 字段4 FROM 表2 where id = 1) as t2,
(SELECT 字段5,字段6 FROM 表3 where id = 1) as t3

-- 注意!合并的结果是每条sql查询结果的笛卡尔积, 所以一般情况下, 最好是每条sql查询结果是一条记录, 这样有利于合并. 不然还是用join比较合适.
```