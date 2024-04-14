# SQL

> **`SQL`对大小写不敏感**
>
> **分号**是在数据库系统中分隔每条 SQL 语句的标准方法

## `select`语句

```sql
select col1,clo2, ... from table_name;
# col1,col2是查询的列名
# table_name是要查询的表名
```

```sql
select distinct col1,col2, ... from table_name;
# distinct 关键词用于返回唯一不同的值
# 例子:
select distinct class from score;
# 从成绩表中返回不同的班级，即查询成绩表中存在那些班级
```



## where子句

> 用于提取满足指定条件的记录

```sql
select col1,col2, ... from table_name where condition;
# 例子
select * from student where student.class = 1;
# 查询1班的所有学生信息
```

在`where`子句中可以使用的运算符

| 运算符  | 描述                       |
| ------- | -------------------------- |
| =       | 等于                       |
| <>      | 不等于                     |
| >       | 大于                       |
| <       | 小于                       |
| >=      | 大于等于                   |
| <=      | 小于等于                   |
| between | 在某个范围内               |
| like    | 搜索某种模式               |
| in      | 指定针对某个列的多个可能值 |

