# MySQL

## 基本命令

### 创建数据库

create database [if not exists ]database_name

### 删除数据库

drop database [if exists ]database_name

### 查看所有数据库名称

show databases;

### 使用数据库

use database_name;

### 创建基本表

create table [if not exists ]database_table_name(
    column_name datatype(length) default defaultValue [primary key]-01
    ...
    [primary key(column_name1，column_name2...)]-02
    );
**注：auto_increment表示自增长，comment表示注释都可以被省略。这些每一列的列名又称字段名，它与表名都可以使用 `来包含用以规避关键字冲突，但是注释comment则必须有单引号进行包含,其中我们的primary key可以单独的声明在每一列中，如01，同时也可以在表创建的结尾声明，除此之外我们此时可以声明多个字段于主键值中从而组成联合主键**

### 显示表格的详细信息

desc table_name;

### 查看表格的创建语句

show create table table_name;

### 字段属性

auto_increment:自增,一般都为主键自增，即当该字段为主键时，该字段的值会自动增加，不需要手动赋值。此称之为主键自增，我们也可以使用unique约束。在不声明主键的情况下建立自增字段。

zerofill:当我们设置了一个数字型的字段，若总长为4，那么当该字段实际值长度仅为1时，其个位以上的数字均以0填充如:'0001'。

not null:当该字段不能为空时,当一个字段被声明为not null后，创建行时，若未设置默认值或者自增那么该字段必须被赋值，否则创建行会失败。

unsigned:当一个字段声明该属性，则该字段必然是一个数字型的数据，此时该字段的值必须是一个正数(否则创建行会失败)，其负数部分添加至整数部分，这也就意味着该字段范围将会扩大一倍。

comment:字段注释属性，当一个字段声明了该属性后，该属性后的字符串将会被视为该字段的注释。但是需要注意的是该字符串需要由单引号进行包含，不可以使用反引号（字段名和表名则可以使用），我们同样可以在创建的表后追加comment属性作为表注释。

**注**：*表格引擎：mysql中的引擎主要使用的是InnoDB和MyISAM两种，InnoDB相较于后者相率较低，空间占用较大(约是后者的两倍大小)，但是其可支持事务处理，数据行锁定，外键约束等操作么人后者则不支持。但是后者支持全文索引，但是InnoDB则不支持，所以前者主要针对的是大型数据库，而后者则只是针对小型数据库。*

### 修改表格的操作

**修改表名:**
alter table old_table_name rename as new_table_name;

**添加字段:**
alter table table_name add column_name column_type [properties];

**修改字段:**
alter table table_name modify column_name column_type [properties];
alter table table_name change old_column-name new_column_name column_type [properties];

**删除字段:**
alter table table_name drop column_name;

**删除数据表:**
drop table [if exists];

**插入数据**
insert into table_name [field1,field2,...] values(value1,value2,...);
**注:**当我们插入数据时，如果没有指定字段(省略字段名)，则默认会按照表中的字段的顺序将这些values插入所有字段，如果指定了字段，则可以仅插入指定的字段。需要注意的是，我们插入的值与字段的类型和范围需要匹配。除此之外，我们的datetime类型的数据应遵循格式：YYYY-MM-DD HH:MM:SS，且置于字符串之中。

**更新数据**
update table_name set field1=value1,field2=value2,... where condition;

**删除数据**
1.delete from table_name where condition; 使用该种方法删除表，并不会重置计数器，这也就意味着新加入的数据的自增值将以删除前的计数器继续进行计算，而且使用该种方法删除的数据是可以被回复的

2.truncate table table_name; 使用该种方法删除数据是用于完全删除该表中的所有数据并会重置计数器，其计数器会被重置，且数据不可被恢复

## 查询数据

基本语法: select fields[*(代表所有字段)] from table_name [where conditions]；

### 特殊的Conditions

#### 取反:在条件中的字段名前使用not关键字对于条件取反->

```sql
select * from table_name where not fieldName =value <===> select * from table_name where not fieldName =value
```

#### is null:当我们想选取某个字段为空时的记录->

```sql
select * from table_name where fieldName is null 
#取反 ->
select * from table_name where fieldName is not null <===> select * from table_name where not fieldName is null
```

#### 选取区间:选取字段值在指定区间内的记录

```sql
select * from table_name where fieldName between value1 and value2(包含value1和value2)
#取反 ->
select * from table_name where not fieldName between value1 and value2
```

#### 选取包含值:选取字段值包含指定值的记录

```sql
select * from table_name where fieldName in (value1,value2,...)
#取反 ->
select * from table_name where not fieldName in (value1,value2,...)
```

#### 模糊查询:通过_和%来模糊查询字段值,其中'_'代表任意一个字符，'%'代表任意多个字符

```sql
select * from table_name where field_name like 'a%' #查询指定字段名以a开头的记录
select * from table_name where field_name like 'a_____' #查询指定字段名以a开头且由五个字符组成的的记录 
```

#### 唯一值查询:使用distinct关键字可以选取唯一的记录，此关键字同一个字段相同的值只会出现一次(此时一般只查询这一个字段)

```sql
select distinct field_name from table_name
```

#### 运算新列：我们对于指定的两个字段进行计算继而获得一个新列，该新列可以通过关键字as进行命名(as可以省略)

```sql
select *,field_name1 operator field_name2 ... as new_field_name from table_name 
```

**注:** *在mysql中加号不可以作为字符串的连接符，如需连接多个字符串可以调用方法concat(str1,str2...):*

```sql
 select concat(ifnull(field_name1,''),'->',ifnull(field_name2,'')) as new_field_name from table_name;
```

#### ifnull(fieldName,replacement)函数:由于在mysql中一个null值加上任意值都会导致结果为null，所以为了避免这一情况我们使用isnull方法将指定值为null时以指定值替。参数含义:如果fieldName为空，则返回replacement，否则返回fieldName

## 排序查询：当我们查询到数据后(所以如果由where条件时，它需要在条件之后)可以使用order by 对于记录进行排序，其中asc为升序(默认选项)，desc为降序

```sql
select * from table_name order by field_name1 asc[,field_name2 asc/desc...];
select * from table_name order by field_name2 desc[,field_name2 desc/asc...];
```

## 聚合函数

使用该函数我们可以进行聚合查询，其中函数的含义如下：

count(filed_name):统计该字段不为空的记录的数量，如果是\*则统计所有输出的记录的数量

```sql
select count(*) as totalCount from table_name;
```

max(filed_name)和min(filed_name)分别获取指定字段的最值，一般用以比较数字型，字符串型会比较字母顺序

```sql
select max(field_name) as field_max from table_name;
```

avg(filed_name) 获取字段的平均值

```sql
select avg(field_name) as field_average from table_name;
```

此时我们依旧可以使用as对其进行命名:
