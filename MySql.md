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

## 查询语句的嵌套及多值赋值

```sql
select fieldName-1,fieldName-2,fieldName-3 from table_1,table_2 where (fieldName-1,fieldName-2) = (select fieldName-1,fieldName-2 from table_3 where fieldName-4 = (select max(fieldName-14) from table-3));
```

## GROUP BY 分组查询:该语句用以将查询后的数据依据某一字段条件进行分组

```sql
 select filed_name1 as new_name1,count(field_name2) new_name2 from table_name where condition group by field_name1;
```

## HAVING 分组查询条件:该语句用以指定分组后的数据的查询条件,不同于where，该语句是在我们进行分组后所进行的筛选，其余where是相反的

```sql
 select filed_name1 as new_name1,count(field_name2) new_name2 from table_name where condition group by field_name1 having condition;
```

## limit startIndex,length:该语句用以指定查询结果的起始位置和长度从而选取指定范围内的数据

```sql
 select * from table_name limit startIndex,length;
```

## 一般正常的sql查询语句应遵循的书写顺序为 select –> from-> where-> group by-> having-> order by->limit

## mySql的约束

**主键约束(primary key)**:该约束的特点保证了数据的唯一性，如果一个表中有多个主键，则必须要求每个主键都是唯一的

**唯一约束(unique)**:当字段的约束为唯一时，该字段的值必须是唯一的，如果该字段的值已经存在，则插入失败

**自增约束(auto_increment)**:该约束的特点是指定字段的值自动增加，每次插入数据时，该字段的值会自动增加，前文已有所介绍

**外键约束(foreign key)**:

1.我们在创建所有字段后可以使用语句 constraint forign_key_name foreign key (foreign_key_field_name) references another_table(refered_field_name[primary key]);

2.或者创建表后使用alter table table_name add constraint foreign_key_name foreign key (foreign_key_field_name) references another_table(refered_field_name[primary key]);

**注:** 当我们使用了外键约束后，那么该表中的 foreign_key_field_name 字段便成为了外键，此时它值必须**存在于**所引用的父表中的refered_field_name,此外它们的**类型必须保持一致**。这个被引用的字段是被引用表的一个主键。

## 多表查询

### 合并结果集 (union,union all)

#### union：将两个查询出的结果合并成一个结果集，其中相同项会被合并， 而union all则会保留两个结果集中的所有项

```sql
select * from table_name1 union select * from table_name2;
select * from table_name1 union all select * from table_name2;
```

但是需要注意的是此时两张表需要保证被合并的两个结果的列数和列类型是一致的。不过合并结果集使用不多。

### 连接查询

所谓连接查询就是把指定的若干表中的若干字段同时显示在同一个表中，这样就可以把多张表的数据合并成一张表。

```sql
select * from table_name1,table_name2...;  #此时两(n)张表中的所有数据全部被合并成一张表
select * from table_name1,table_name2... where conditions;  #此时两(n)张表中的符合条件全部记录的全部数据被合并成一张表
select table_name1.field_name1,table_name1.field_name2,...table_name2.field_name1,table_name2.field_name2... from table_name1,table_name2... where conditions;  #此时两(n)张表中的符合条件的记录中的指定字段被合并成一张表
select t1.field_name1,t1.field_name2,...t2.field_name1,t2.field_name2... from t2.field_name1 (as) t1,field_name2 (as) t2... where conditions;  #与前者是同样的操作，只是为表格添加了别名以方便书写
```

## 内连接

前面的连接查询实际上就是内连接，但是不是标准的sql语句，是mySql的特有语法，对于标准的sql语法来说应当写作:

```sql
select * from table_name1 inner join table_name2 on conditions; #注意这里的conditions的连接词为on而不再是where了
```

## 外连接：事实上我们注意到我们实现内连接是建立在两张表的公用数据之上的，这里经常为主键和外键，而假设含主键的这张表存在一个数据，它的主键在另一张表中是不存在的，因为我们知道我们的外键是基于所引用的表的主键的，当该主键不存在时，我们无法创建这个外键，但是拖过存在这个主键去不意味着我们就一定存在这个外键，会因此，有可能一条数据在含主键的这张表存在，但是在含外键的这张表上不存在相应的外键与之对应，则该数据同样无法显示，如

```sql
select * from table_name1 inner join table_name2 on conditions
此时若table1的数据为：
id student_name age
01 Rongxin   21
02 Griffin   22
03 Jack      21
而table2的数据为：
id score
01 95
02 98
```

此时所选取的结果中将不含03这个学生的信息，因为table2中没有03这个学生的信息，所以我们可以使用外连接来解决这个问题：

### 左外连接:使用该连接我们可以将以左翼的表格为主体，此时即使table2中没有该段数据，该段数据将依旧会被展示，如前面的两个表，其结果将不再跳过03该段数据，至于此时table2位置的数据因为为空，则全部字段都将显示为空值

```sql
select * from table_name1 left join table_name2 on conditions;
```

### 右外连接:该连接与前者左连接功能是一致的，只是此时讲以右翼的表格作为主表

```sql
select * from table_name1 right join table_name2 on conditions;
```

### 自然连接:使用该连接，我们无需给出两张表的主外键关系等式，因为其会对两张表进行自动查询。如两张连接的表中名称和类型完全一致的列作为条件，例如t1和t2表都存在field1列，并且类型一致，所以会被自然连接找到

```sql
select * from table_name1 natural join table_name2
select * from table_name1 natural left join table_name2
select * from table_name1 natural right join table_name2
```

### 数据库的导出和恢复

我们可以在命令行下输入mysqldump -uroot -ppassword database_name > backup.sql来导出数据库，然后在命令行下输入mysql -uroot -ppassword database_name < backup.sql来导入数据库，这样就可以完全恢复数据库了，不过需要注意的是，导出的sql并不包含创建数据库的操作，所在我们导入之前需要提前创建出一个数据库

亦或者我们可以在msql内部进行该导入操作:source path\backup.sql

## 事务(transaction)

事务即使一段命令，而mysql默认是自动提交的，因此我们的任意一条指令都是自动提交的事务，然而如果我们使用的时InnoDB或者BDB引擎的表格，则也可以实现批量提交的事务，也就是在多条指令执行完成后统一使用commit命令提交事务，如果我们使用的是MyISAM引擎的表格，则不支持该操作。在使用事务之前我们首先需要使用set autocommit=0以关闭事物的自动提交，而后使用start transaction开始事务，随后我们便可以使用sql语句进行操作，如果所有事务都执行成功(此时事务并未真正执行)，我们可以使用commit提交，此时所有事务将同时执行完成;如果其中有一条语局执行出错则应该使用rollback进行回滚，以回到初始状态。注意事务执行完成后应当使用set autocommit=1以开启事务的自动提交。
