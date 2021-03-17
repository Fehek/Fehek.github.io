---
title: web前端学习笔记12——数据库
subtitle: 数据库
abbrlink: b1456509
tags:
  - web前端
  - 数据库
categories: web前端学习笔记
date: 2021-03-09 10:02:00
index_img:
banner_img:
---

# 数据库
## 关系型数据库 RDBMS
可以理解为Excel
里面都是表，只是用程序代码来操作表格里的数据
- CS模型(client/server)
  MiaraDB
  PostgreSQL
  Office Access
  SQLServer
  OceanDB
  Oracle
    MySQL
- 非CS模型
  SQLite 10亿行，4T
    sqlite把数据存在一个文件里，通过sqlite提供的相关函数去读取

## 非关系型数据库
- 文档型数据库(MongoDB, PunchDB)
`JOSN {name: 'z', age: 18, foo: 8}, {name: 'a', age: 28}`
- 缓存型数据库，KV数据库，Key, Value
Redis
redis.get('xxx')
redis.push('yyy', '333')
- 日志型数据库

# 语句
1. select 表头关键字（展示哪几列）
&nbsp;&nbsp;&nbsp;&nbsp;`aaa * 10 as bbb`，xxx是新建列的名称；若有空格，用''包起来；关键字distinct，不重复
2. from 文件夹
3. where 筛选条件
- 运算符
  - and or not
  - in
  `state = 'VA' or state = 'GA' or state = 'FL'`和`state in ('VA', 'FL', 'GA')`等价
  - between
  `points >= 1000 and points <= 300`和`points between 1000 and 3000`等价
  - like
  `last_name like '%y'` 筛选姓最后一个字母是y的人
  % 任意字符数&nbsp;&nbsp;&nbsp;&nbsp;_ 单字符
  - regexp 正则表达式
  ```md
  ^       开头
  $       结尾
  |       逻辑或
  [abc]   abc其中一个或多个
  [a-f]   a-f中一个或多个
  ```
  详情见[这里](./468b9935.html#正则表达式)
  - is null 为空的单元格
4. ordered by 排序关键字
在关系型数据库中，每个表格都有一个主键列，这一列中的值能够唯一识别表里的记录
关键字desc 降序排列
5. limit
`limit 10` 只展示前10项
`limit 6，3` 略过前6项，展示3项

# 连接
内连接join
```sql
from a
join b on a.id = b.id
-- 用相同id来合并a和b
-- join b using(id)
也可用using来连接，多个连接条件中间加,

from employees e
join employees m
  on e.reports_to = m.employee_id
-- 自连接 
```
外连接left join/right join
```sql
from c
left join o on c.id = o.id
-- 所有左表的记录被返回，不管条件正确还是错误
-- right join则是右表都被返回
```
natural join
cross join
union 合并多个查询的结果
```sql
select...
from...
where...
union
select...
from...
where...
```

# 行
```sql
-- 插入单行
insert into xxx(addr, n, who)
values(default, 1, 'xxx')

-- 插入多行
insert into xxx(name)
values('wwr'),('tn'),('io')

-- 复制一张表
create table orders_archived as
select * from orders

-- 更新行
update salary
 set gender = f
where id > 10

-- 删除行
delete from xxx
where...
```

# 函数
- 基本函数
`max() min() avg() sum() count()` 
- [group by](https://www.w3school.com.cn/sql/sql_groupby.asp) 分组
- [having](https://www.w3school.com.cn/sql/sql_having.asp)
- case
```sql
-- 简单case函数
case gender
  when '1' then 'f'
  when '2' then 'm'
  else 'NaN' end

-- case搜索函数
case when gender = '1' then 'f'
     when gender = '2' then 'm'
     else 'NaN' end

-- case函数只返回第一个符合条件的值，剩下的case部分将会被自动忽略
--下面这段sql，无法得到“第二类”这个结果
case when col_1 in ('a','b') then '第一类'
     when col_1 in ('a') then '第二类'
     else '其他' end
```
