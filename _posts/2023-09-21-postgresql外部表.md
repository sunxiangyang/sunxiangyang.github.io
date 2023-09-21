---
layout:     post   				    # 使用的布局（不需要改）
title:      postgresql外部表 				# 标题 
date:       2023-09-21 				# 时间
author:     stq 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
     postgresql
---
外部表的优势
```
数据集成和访问：

多数据源访问：外部表允许你在一个数据库中访问多个不同数据源的数据，包括其他数据库、文件、Web服务等。
集成异构数据：你可以将来自不同数据源的数据统一到一个数据库中，使其更容易查询和分析。
数据仓库集成：外部表可以用于将数据仓库与外部数据源集成，从而为分析提供更广泛的数据。
性能优化：

节省存储空间：外部表可以避免将外部数据复制到本地数据库，从而节省存储空间。
性能优化：数据库优化器可以通过选择性地推送查询计划到外部数据源来优化查询性能，减少数据传输的成本。
数据保持一致性：

实时或定期同步：你可以定期同步或实时访问外部数据源，以确保本地数据库中的数据保持与外部源的一致性。
灵活性：

架构更改：如果外部数据源的架构发生更改，你可以更轻松地适应这些变化，而无需修改本地数据库架构。
数据格式支持：外部表通常支持多种数据格式，如CSV、JSON、XML等，使你能够处理多样化的数据。
节省时间和成本：

数据移动和ETL减少：外部表减少了将数据从一个系统复制到另一个系统的需求，从而减少了ETL（提取、转换、加载）的工作量。
维护成本降低：通过在一个地方维护数据，而不是多个副本，降低了数据维护的成本。
大规模数据处理：

大数据支持：外部表是处理大规模数据的有效方式，因为它们可以在需要时延迟加载数据，而不是将整个数据集加载到内存中。
跨平台支持：

跨数据库支持：一些外部表实现允许在不同类型的数据库之间建立连接，例如在PostgreSQL中使用postgres_fdw连接到另一个PostgreSQL数据库。
```
以下以postgresql中如何添加一个postgres外部表距离如何使用
激活postgres_fdw拓展
```sql
CREATE EXTENSION postgres_fdw;
```

创建外部服务器对象：在本地数据库中创建一个外部服务器对象，以便连接到远程PostgreSQL数据库。使用CREATE SERVER语句来创建服务器对象
```sql
CREATE SERVER remote_server
FOREIGN DATA WRAPPER postgres_fdw
OPTIONS (host 'remote_host', dbname 'remote_database', port 'remote_port');

```

创建外部用户映射：为了在本地数据库和远程服务器之间建立连接，需要创建一个外部用户映射。使用 CREATE USER MAPPING 语句创建映射：

```sql
CREATE USER MAPPING FOR local_user
SERVER remote_server
OPTIONS (user 'remote_user', password 'remote_password');

```
创建外部表：现在可以创建外部表，该表会连接到远程PostgreSQL数据库的表。使用 CREATE FOREIGN TABLE 语句创建外部表:
```sql
CREATE FOREIGN TABLE external_table (
    column1 data_type,
    column2 data_type,
    ...
)
SERVER remote_server
OPTIONS (schema_name 'remote_schema', table_name 'remote_table');

```
现在就可以像查询本地表一样查询外部表了






