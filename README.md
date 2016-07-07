# 版本说明

本版本专用于定制 mysqldump，增加如下功能：

 - 导出表数据时，每导出10万行打印一次进度。 
 - 增加指定初始化 SQL 功能，建立连接后首先执行指定 SQL: --init-sql="sql1;sql2;sql3"，注意：sql1、sql2 等不能是SELECT、SHOW、DESC等有结果集的SQL，可以是DDL、INSERT、REPLACE、DELETE、USE、SET、ALTER SYSTEM 等没有结果集的 SQL

# 编译

```
  git clone https://github.com/raywill/mysql-server/;
  cd mysql-server;
  git checkout 5.6;
  mkdir build;
  cd build;
  cmake ..;
  make -j30
```

其中，mysqldump 的源码位于 https://github.com/raywill/mysql-server/blob/5.6/client/mysqldump.c

# 用法

确认使用了正确的mysqldump, 末尾应该有 **Customized Edition** 字样

```bash
  ./client/mysqldump --version
  Ver 10.13 Distrib 5.6.31, for Linux (x86_64), Customized Edition
```

** 标准用法 **

```bash
  ./client/mysqldump -h 127.0.0.1 -P 3066 -u123456 test
```

** 定制用法 **

```baseh
  # 不导出 triggers, 建立连接后立即执行 SET @@session.v1=1;SET @@session.v2=2; 两条 SQL 语句。注意：每一条语句末尾都必须有分号。
  ./client/mysqldump -h 127.0.0.1 -P 3066 -u123456 --triggers=false --init-sql="SET @@session.v1=1;SET @@session.v2=2;" test 
```

