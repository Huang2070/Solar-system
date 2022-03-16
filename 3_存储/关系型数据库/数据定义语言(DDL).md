* 查询列: show columns from 表名;
* 查询索引: show keys from 表名;
* 查看表的字段信息：desc 表名;
* 查看表的所有信息：show create table 表名;
* 添加主键约束：alter table 表名 add constraint 主键 （形如：PK_表名） primary key 表名(主键字段);
* 添加外键约束：alter table 从表 add constraint 外键（形如：FK_从表_主表） foreign key 从表(外键字段) references 主表(主键字段);
* 删除主键约束：alter table 表名 drop primary key;
* 删除外键约束：alter table 表名 drop foreign key 外键（区分大小写）;
* 修改表名：alter table t_book rename to bbb;
* 添加列：alter table 表名 add column 列名 varchar(30);
* 删除列：alter table 表名 drop column 列名;
* 修改列名MySQL： alter table bbb change nnnnn hh int;
* 修改列名SQLServer：exec sp_rename't_student.name','nn','column';
* 修改列名Oracle：alter table bbb rename column nnnnn to hh int;
* 修改列属性：alter table t_book modify name varchar(22);
sp_rename：SQLServer 内置的存储过程，用与修改表的定义。
