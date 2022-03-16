# 时间函数

## now() 
获得当前日期+时间（date + time）
```sql
select now();
```

## sysdate()
动态获得当前日期+时间（date + time）
```sql
# sysdate() 日期时间函数跟 now() 类似，不同之处在于：now() 在执行开始时值就得到了，sysdate() 在函数执行时动态得到值。
select now(), sleep(3), now();
select sysdate(), sleep(3), sysdate();
```

## current_timestamp()
获得当前时间戳
```sql
select current_timestamp;
select current_timestamp();
```
>mysql的时间标准格式就是“年-月-日 时-分-秒”, 通过这个格式, mysql可以识别date字段.

## date_format()
日期/时间转换为字符串
```sql
# date_format(date,format), time_format(time,format) 能够把一个日期/时间转换成各种各样的字符串格式.

select date_format('2008-08-08 22:23:01', '%Y%m%d%H%i%s');
```

## str_to_date()
字符串转换为日期
```sql
# 第一个参数是时间字符串, 第二个参数标记字符串的格式, str_to_date自动转换为date类型的标准时间格式.

select str_to_date('08/09/2008', '%m/%d/%Y'); -- 2008-08-09
select str_to_date('08/09/08' , '%m/%d/%y'); -- 2008-08-09
select str_to_date('08.09.2008', '%m.%d.%Y'); -- 2008-08-09
select str_to_date('08:09:30', '%h:%i:%s'); -- 08:09:30
select str_to_date('08.09.2008 08:09:30', '%m.%d.%Y %h:%i:%s'); -- 2008-08-09 08:09:30
```

## to_days()/from_days()
计算从公元0年的天数
```sql
# 从0000-00-00到参数值有多少天
select to_days('2022-01-01'); -- 738521
# 从0000-00-00加上参数天数是那一天
from_days(738521); -- 2022-01-01
```

## unix_timestamp()
Unix时间戳、日期转换函数 
```sql
select unix_timestamp(); -- 1218290027
select unix_timestamp('2008-08-08'); -- 1218124800
select unix_timestamp('2008-08-08 12:30:00'); -- 1218169800

select from_unixtime(1218169800); -- '2008-08-08 12:30:00'

select from_unixtime(1218169800, '%Y %D %M %h:%i:%s %x'); -- '2008 8th August 12:30:00 2008'
```

## adddate()
为日期增加一个时间间隔
adddate(), addtime()和date_add()功能相同, 可以使用date_add()代替.
```sql
set @dt = '2008-08-09 12:12:33';

select date_add(@dt, interval 1 day);
select date_add(@dt, interval 1 hour);
select date_add(@dt, interval 1 minute);
select date_add(@dt, interval 1 second);
select date_add(@dt, interval 1 microsecond);
select date_add(@dt, interval 1 week);
select date_add(@dt, interval 1 month);
select date_add(@dt, interval 1 quarter);
select date_add(@dt, interval 1 year);

select date_add(@dt, interval -1 day);

select date_add(@dt, interval '01:15:30' hour_second);
select date_add(@dt, interval '1 01:15:30' day_second);
```

## datediff()/timediff()
日期、时间相减函数
```sql
#datediff(date1,date2)：两个日期相减 date1 - date2，返回天数。
select datediff('2008-08-08', '2008-08-01'); -- 7;
select datediff('2008-08-01', '2008-08-08'); -- -7

#timediff(time1,time2)：两个日期相减 time1 - time2，返回 time 差值。timediff函数的两个参数类型必须相同.
select timediff('2008-08-08 08:08:08', '2008-08-08 00:00:00'); -- 08:08:08
select timediff('08:08:08', '00:00:00'); -- 08:08:08
```

# 字符串处理函数
## left()
获取字符串字段的前n位
```sql
select left( `字段名`, 5) as 新字段名 from 表名;
```


# 逻辑函数
## IFNULL(expr1,expr2)
如果expr1不是NULL，IFNULL()返回expr1，否则它返回expr2。

```sql
select IFNULL(1,0); --1
select IFNULL(0,10); --0
select IFNULL(1/0,10); --10
select IFNULL(1/0,'yes'); --yes
```

## IF(expr1,expr2,expr3) 
如果expr1是TRUE(expr1<>0且expr1<>NULL)，那么IF()返回expr2，否则它返回expr3。

```sql
select IF(1>2,2,3); --3   
select IF(1<2,'yes','no'); --yes
```
