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


## replace into
REPLACE的运行与INSERT很相似。只有一点例外，假如表中的一个旧记录与一个用于PRIMARY KEY或一个UNIQUE索引的新记录具有相同的值，则在新记录被插入之前，旧记录被删除。 
注意，除非表有一个PRIMARY KEY或UNIQUE索引，否则，使用一个REPLACE语句没有意义。

REPLACE INTO `table` (`unique_column`,`num`) VALUES ('$unique_value',$num);跟INSERT INTO `table` (`unique_column`,`num`) VALUES('$unique_value',$num) ON DUPLICATE UPDATE num=$num;还是有些区别的.
区别就是replace into的时候会删除老记录。如果表中有一个自增的主键,那么就要出问题了。

首先，因为新纪录与老记录的主键值不同，所以其他表中所有与本表老数据主键id建立的关联全部会被破坏。
其次，就是，频繁的REPLACE INTO会造成新纪录的主键的值迅速增大。

