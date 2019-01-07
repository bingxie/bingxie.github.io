---
layout: post
title: 使用 Rails Migration转换 MySQL数据库和表的字符集总结
categories: [Tech]
tags: [rails, mysql]
---

![](/images/Bing_712.JPG)

## MySQL Character Set基础知识

对于 MySQL 数据库你可以在不同的 Level 设置Character Set 和 Collation，包括：Server Level，Database Level，Table Level，Column Level 还有 Application Level.

### Server Level:

可以通过命令行设置，也可以通过配置文件设置

默认： `--character-set-server=latin1`

`latin1_swedish_ci` is the default collation for `latin1`

还可以通过重新编译时指定参数实现：`use the DEFAULT_CHARSET and DEFAULT_COLLATION`

作用范围：如果创建数据库时不指定，那么就使用 Server Level 的设置

查看当前的设定，可以查看系统变量：`character_set_server and collation_server`


### Database Level:

可以在创建数据库时设置：

```sql
CREATE DATABASE db_name CHARACTER SET latin1 COLLATE latin1_swedish_ci;
```

默认值：可以由 `character_set_database` and `collation_database` 系统变量决定.

可以通过下面的命令查看当前的设置:

```sql
USE db_name;

SELECT @@character_set_database, @@collation_database;
```

或者

```sql
SELECT DEFAULT_CHARACTER_SET_NAME, DEFAULT_COLLATION_NAME
FROM INFORMATION_SCHEMA.SCHEMATA WHERE SCHEMA_NAME = 'db_name';
```

作用：如果建表时没有指定，那么会作为表的默认值，同时也是作为 LOAD DATA 的默认值

#### 修改数据库level 的 character set 和 collation

```sql
ALTER DATABASE db_name CHARACTER SET utf8 COLLATE utf8_unicode_ci;
```

或者单独修改

```sql
ALTER DATABASE my_database DEFAULT COLLATE utf8_unicode_ci;

ALTER DATABASE my_database DEFAULT CHARACTER SET utf8;
```

### Table Level:

可以在建表的语句中进行设置

作用：如果字段没有具体制定，那么会作为字段的默认值
note：该功能是 mysql 的一个扩展，不是标准的SQL

使用下面语句可以同时修改 table 和 table 中字段的设置

```sql
ALTER TABLE tbl_name CONVERT TO CHARACTER SET charset_name [COLLATE collation_name];
```

查看某个数据库中的所有表的一些设置信息的语句

```sql
SHOW TABLE STATUS FROM db_name;
```


### Column Level:

N/A

### Application Connection Level:

对于 Rails 应用，在 database.yml的数据库连接设置中加上 `?reconnect=true&encoding=utf8&collation=utf8_unicode_ci`

#### 查看数据库不同级别的元数据的设置的语句，比如：character set

```sql
SELECT DEFAULT_COLLATION_NAME FROM information_schema.SCHEMATA S WHERE schema_name = 'db_name' AND DEFAULT_COLLATION_NAME != 'utf8_unicode_ci';

SELECT TABLE_NAME, TABLE_COLLATION FROM information_schema.TABLES WHERE table_schema = 'db_name' AND table_collation != 'utf8_unicode_ci';

SELECT * FROM information_schema.COLUMNS WHERE table_schema = 'db_name' AND collation_name != 'utf8_unicode_ci';

```

### ConvertDatabaseCharacterSetAndCollationToUtf8 Migration
```ruby
class ConvertDatabaseCharacterSetAndCollationToUtf8 < ActiveRecord::Migration
  def up
    execute <<~SQL
      ALTER DATABASE #{ActiveRecord::Base.connection.current_database} CHARACTER SET utf8 COLLATE utf8_unicode_ci;
    SQL
  end

  def down
    execute <<~SQL
      ALTER DATABASE #{ActiveRecord::Base.connection.current_database} CHARACTER SET latin1 COLLATE latin1_swedish_ci;
    SQL
  end
end
```

### ConvertTablesCharacterSetAndCollationToUtf8 Migration
```ruby
class ConvertTablesCharacterSetAndCollationToUtf8 < ActiveRecord::Migration
  def up
    execute("SET foreign_key_checks = 0")

    latin_tables_sql = <<~SQL
      SELECT TABLE_NAME, TABLE_COLLATION
      FROM information_schema.TABLES
      WHERE table_schema = '#{ActiveRecord::Base.connection.current_database}'
      AND table_collation != 'utf8_unicode_ci';
    SQL

    results = ActiveRecord::Base.connection.execute(latin_tables_sql)
    say "Total: #{results.count}"

    results.each do |result|
      alter_table_sql = "ALTER TABLE #{result[0]} CONVERT TO CHARACTER SET utf8 COLLATE utf8_unicode_ci;"
      execute alter_table_sql
    end

    execute("SET foreign_key_checks = 1")
  end
end
```


### Rails Migration tips
在调试该功能的时候，学到的一些 tips：

`rake db:migrate:status`  查看当前migration 的状态，包括版本信息等

`rake db:migrate VERSION=33333333` migrate指定 version

`rake db:rollback STEP=n` 通过 STEP 参数指定回滚的范围

`User.connection` 可以用来检查当前数据库设置和连接的信息

`ActiveRecord::Base.connection.current_database` 获取当前连接的数据库

`ActiveRecord::Migrator.current_version`  查看当前的版本

在 Rails Console 或者 Runner 中执行 SQL语句，可以使用 `ActiveRecord::Migration.execute("SQL")`

---
参考文章：
  [How to change all columns' and tables' collation to 'utf8_bin' in MySQL](https://confluence.atlassian.com/jirakb/how-to-change-all-columns-and-tables-collation-to-utf8_bin-in-mysql-601456761.html)
