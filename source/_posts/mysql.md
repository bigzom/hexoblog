---
title: mysql
date: 2019-08-22 11:19:38
tags: [数据库]
---
### 存储引擎
采用不同的技术存储数据（mysql独有）
<!-- more -->
可以使用show engines命令查看数据库有的存储引擎
- __指定存储引擎__
1. 建表时指定
      Creat table table_name(
          结构
      )ENGINE = 引擎名称;
2. 修改
      Alter table table_name ENGINE = 引擎名称；
3. 查询表结构（能看到存储引擎）
      show create table table_name;
- __常用存储引擎__
MyISAM:节省数据库空间，当读取远大于修改时使用。
InnorDB:支持事务,对数据修改频繁时可以使用。
Memory:储存在内存中，用于保存非永久性数据，速度快
### 事务(transactiopn)
可以保证多个操作原子性。添加、删除等操作，要么全部成功要么全部失败
事务完成后mysql会自动提交不可回滚
关闭事务：start transactiopn;
### 事务的隔离级别
- read uncommited 读未提交
可以读未提交的数据（脏数据）
- read commited 读已提交
提交后才能读取到，但是可能出现两次读取数据不一致（不可重复读,强调更新）
- repeat read 重复读（默认隔离级别）
可能出现幻读（强调插入）
- seralizable 串行化
同一时间只能有一个事务操作数据库，效率太低
- 查看当前隔离级别
select @@tx_isolation;