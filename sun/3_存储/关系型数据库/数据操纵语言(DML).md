## update中使用case when
```sql
update test
set fjjtzlx =
(
    case fjjtzlx
    when '优化型' then 'YHX'
    when '强化收益型' then 'QHSYX'
    when '债券型' then 'ZQX'
    when '货币型' then 'HBX'
    when '收益型' then 'SYX'
    when '指数优化型' then 'ZSYHX'
    when '增强型' then 'ZQX'
    else fjjtzlx end 
)
```
这样的用法只是局限于要更新的列值不是很多.