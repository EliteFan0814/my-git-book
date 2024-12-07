# 数据库

1. 提供结构化数据的持久存储
2. 索引保证数据查询速度
3. 事务的原子性保证数据不丢失

## 常用 SQL 命令

### create database 创建数据库

```sql
create database database_name;
```

### create table 创建一个新的数据表

```sql
create table table_name(column1Str datatype,column2Str datatype,primary key(one/more columnStr))
```

### insert into 向表中插入新的数据

```sql
insert into table_name(column1,column2)values(value1,value2);
```

### delete 从表中删除数据

```sql
delete from table_name where {codition};
```

### update 更新表中的数据

```sql
update table_name set column1 = value1,column2 = value2 [where condition];
```

### select 从数据库中查询或选取数据

```sql
select column1,column2 from table_name;
```

### alter table 修改数据表

```sql
-- 修改数据表的字段
alter table table_name {add|drop|modify| column_name{data_type}};
-- 修改数据表的名称
alter table table_name rename to new_table_name;
```

###  删除数据表
1. drop table 删除表，数据和表结构一起删除，快速
2. truncate table 删除所有数据，保留表结构，不能撤销还原
3. delete from 是逐行删除速度极慢，不适合大量数据删除
```sql
drop table table_name;
truncate table table_name;
```

### create/drop index 创建/删除索引

```sql
-- 创建索引
create unique index index_name on table_name(column1,column2);
-- 删除索引
alter table table_name drop index index_name;
```
