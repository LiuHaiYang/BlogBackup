---
title: TiDB入门-3
date: 2021-05-11 21:18:42
tags: 数据库
---

## TiDB 5.0

5.0 版本中，专注数据库快速构建应用程序，无需担心性能，性能抖动，安全，高可用，容灾，SQL语句的性能问题排查问题。

在 5.0 版本中，可以获得以下关键特性：

- TiDB 通过 TiFlash 节点引入了 MPP 架构。这使得大型表连接类查询可以由不同 TiFlash 节点共同分担完成。
- 引入聚簇索引功能，提升数据库的性能。例如，TPC-C tpmC 的性能提升了 39%。
- 开启异步提交事务功能，降低写入数据的延迟。例如：Sysbench 设置 64 线程测试 Update index 时, 平均延迟由 12.04 ms 降低到 7.01ms ，降低了 41.7%。
- 通过提升优化器的稳定性及限制系统任务对 I/O、网络、CPU、内存等资源的占用，降低系统的抖动。例如：测试 8 小时，TPC-C 测试中 tpmC 抖动标准差的值小于等于 2%。
- 通过完善调度功能及保证执行计划在最大程度上保持不变，提升系统的稳定性。
- 引入 Raft Joint Consensus 算法，确保 Region 成员变更时系统的可用性。
- 优化 EXPLAIN 功能、引入不可见索引等功能帮助提升 DBA 调试及 SQL 语句执行的效率。
- 通过从 TiDB 备份文件到 Amazon S3、Google Cloud GCS，或者从 Amazon S3、Google Cloud GCS 恢复文件到 TiDB，确保企业数据的可靠性。
- 提升从 Amazon S3 或者 TiDB/MySQL 导入导出数据的性能，帮忙企业在云上快速构建应用。例如：导入 1TiB TPC-C 数据性能提升了 40%，由 254 GiB/h 提升到 366 GiB/h。

### 新功能

- List 分区表 (List Partition)
- List COLUMNS 分区表 (List COLUMNS Partition)
- 不可见索引（Invisible Indexes）
- EXCEPT 和 INTERSECT 操作符
- 事务
- 字符集和排序规则

## 基本功能
### 数据类型
- 数值类型：BIT、BOOL|BOOLEAN、SMALLINT、MEDIUMINT、INT|INTEGER、BIGINT、FLOAT、DOUBLE、DECIMAL。
- 日期和时间类型：DATE、TIME、DATETIME、TIMESTAMP、YEAR。
- 字符串类型：CHAR、VARCHAR、TEXT、TINYTEXT、MEDIUMTEXT、LONGTEXT、BINARY、VARBINARY、BLOB、TINYBLOB、MEDIUMBLOB、LONGBLOB、ENUM、SET。
- JSON 类型。

### 运算符
- 算术运算符、位运算符、比较运算符、逻辑运算符、日期和时间运算符等。

### 字符集及排序规则

- 字符集：UTF8、UTF8MB4、BINARY、ASCII、LATIN1。
- 排序规则：UTF8MB4_GENERAL_CI、UTF8MB4_UNICODE_CI、UTF8MB4_GENERAL_BIN、UTF8_GENERAL_CI、UTF8_UNICODE_CI、UTF8_GENERAL_BIN、BINARY。

### SQL 语句
- 完全支持标准的 Data Definition Language (DDL) 语句，例如：CREATE、DROP、ALTER、RENAME、TRUNCATE 等。
- 完全支持标准的 Data Manipulation Language (DML) 语句，例如：INSERT、REPLACE、SELECT、Subqueries、UPDATE、LOAD DATA 等。
- 完全支持标准的 Transactional and Locking 语句，例如：START TRANSACTION、COMMIT、ROLLBACK、SET TRANSACTION 等。
- 完全支持标准的 Database Administration 语句，例如：SHOW、SET 等。
- 完全支持标准的 Utility 语句，例如：DESCRIBE、EXPLAIN、USE 等。
- 完全支持 SQL GROUP BY 和 ORDER BY 子语句。
- 完全支持标准 SQL 语法的 LEFT OUTER JOIN 和 RIGHT OUTER JOIN。
- 完全支持标准 SQL 要求的表和列别名。

### 分区表
- 支持 Range 分区。
- 支持 Hash 分区。

### 视图
- 支持普通视图。

### 约束
- 支持非空约束。
- 支持主键约束。
- 支持唯一约束。


## 与 MySQL 兼容性对比

TiDB 100% 兼容 MySQL 5.7 协议、MySQL 5.7 常用的功能及语法。

### 不支持的功能特性
- 存储过程与函数
- 触发器
- 事件
- 自定义函数
- 外键约束 
- 临时表
- 全文/空间函数与索引
- 非 ascii/latin1/binary/utf8/utf8mb4 的字符集
- SYS schema
