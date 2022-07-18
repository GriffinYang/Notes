# MySQL

## 基本命令

创建数据库：create database database_name;
创建基本表：create table table_name(
    column_name datatype(length) defaultValue(not null/default null) [auto_increment] [comment]
    ...
    [primary key(column_name)]
    );
**注：auto_increment表示自增长，comment表示注释都可以被省略。这些每一列的列名又称字段名，它与表名都可以使用 `来包含用以规避关键字冲突，但是注释comment则必须有单引号进行包含**
