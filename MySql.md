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
