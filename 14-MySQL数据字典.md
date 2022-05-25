# Chapter 14 MySQL Data Dictionary

# 章节 14 MySQL 数据字典

**Table of Contents**

**目录**

* [14.1 Data Dictionary Schema 数据字典模式](./14-MySQL数据字典/1-数据字典模式.md)
* [14.2 Removal of File-based Metadata Storage 移除的基于文件的元数据存储](./14-MySQL数据字典/2-移除的基于文件的元数据存储.md)
* [14.3 Transactional Storage of Dictionary Data 数据字典的事务存储](./14-MySQL数据字典/3-数据字典事务存储.md)
* [14.4 Dictionary Object Cache 字典对象缓存](./14-MySQL数据字典/4-字典对象缓存.md)
* [14.5 INFORMATION_SCHEMA and Data Dictionary Integration INFORMATION_SCHEMA 和数据字典整合](./14-MySQL数据字典/5-INFORMATION_SCHEMA和数据字典整合.md)
* [14.6 Serialized Dictionary Information (SDI) 序列化字典信息（SDI）](./14-MySQL数据字典/6-序列化字典信息（SDI）.md)
* [14.7 Data Dictionary Usage Differences 数据字典使用差异](./14-MySQL数据字典/7-数据字典使用差异.md)
* [14.8 Data Dictionary Limitations 数据字典限制](./14-MySQL数据字典/8-数据字典限制.md)


MySQL Server incorporates a transactional data dictionary that stores information about database objects. In previous MySQL releases, dictionary data was stored in metadata files, nontransactional tables, and storage engine-specific data dictionaries.

MySQL 服务器包含一个事务数据字典，它存储有关数据库对象信息的。在以前的 MySQL 发行版，数据字典存储在元数据文件，非事务表和存储引擎特定的数据字典中。

This chapter describes the main features, benefits, usage differences, and limitations of the data dictionary. For other implications of the data dictionary feature, refer to the “Data Dictionary Notes” section in the [MySQL 8.0 Release Notes](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/).

本章节介绍数据字典的主要特性，好处，用法差异和限制。有关数据字典特性的其他可能的影响，参阅 MySQL 8.0 发行版说明中的数据字典说明。

Benefits of the MySQL data dictionary include:

MySQL 数据字典包含的好处：

* Simplicity of a centralized data dictionary schema that uniformly stores dictionary data. See [Section 14.1, “Data Dictionary Schema”](https://dev.mysql.com/doc/refman/8.0/en/data-dictionary-schema.html).

  统一存储数据字典的集中式数据字典模式的简单性。参考小节 14.1 数据字典模式

* Removal of file-based metadata storage. See [Section 14.2, “Removal of File-based Metadata Storage”](https://dev.mysql.com/doc/refman/8.0/en/data-dictionary-file-removal.html).

  移除的基于文件的元数据存储。参考小节 14.2 移除的基于文件的元数据存储

* Transactional, crash-safe storage of dictionary data. See [Section 14.3, “Transactional Storage of Dictionary Data”](https://dev.mysql.com/doc/refman/8.0/en/data-dictionary-transactional-storage.html).

  事务的，奔溃安全的数据字典存储。参考小节 14.3 数据字典事务存储

* Uniform and centralized caching for dictionary objects. See [Section 14.4, “Dictionary Object Cache”](https://dev.mysql.com/doc/refman/8.0/en/data-dictionary-object-cache.html).

  统一的和集中的字典对象缓存。参考小节 14.4 字典对象缓存

* A simpler and improved implementation for some [`INFORMATION_SCHEMA`](https://dev.mysql.com/doc/refman/8.0/en/information-schema.html) tables. See [Section 14.5, “INFORMATION_SCHEMA and Data Dictionary Integration”](https://dev.mysql.com/doc/refman/8.0/en/data-dictionary-information-schema.html).

  一些 INFORMATION_SCHEMA 表更简单和改进的实现。参考小节 14.5 INFORMATION_SCHEMA 和数据字典整合

* Atomic DDL. See [Section 13.1.1, “Atomic Data Definition Statement Support”](https://dev.mysql.com/doc/refman/8.0/en/atomic-ddl.html).

  原子的 DDL。参考小节 13.1.1 原子的数据定义语句支持

> **Important**
>
> **重要**
>
> A data dictionary-enabled server entails some general operational differences compared to a server that does not have a data dictionary; see [Section 14.7, “Data Dictionary Usage Differences”](https://dev.mysql.com/doc/refman/8.0/en/data-dictionary-usage-differences.html). Also, for upgrades to MySQL 8.0, the upgrade procedure differs somewhat from previous MySQL releases and requires that you verify the upgrade readiness of your installation by checking specific prerequisites. For more information, see [Section 2.11, “Upgrading MySQL”](https://dev.mysql.com/doc/refman/8.0/en/upgrading.html), particularly [Section 2.11.5, “Preparing Your Installation for Upgrade”](https://dev.mysql.com/doc/refman/8.0/en/upgrade-prerequisites.html).
>
> 一个支持数据字典的服务器同一个没有支持数据字典的服务器需要一些通用的操作差异；参考小节 14.7 数据字典用法差异。另外，对于 MySQL 8.0 的升级，升级过程也跟之前的 MySQL 发行版有些不同，这要求你通过检查特定的先决条件来确认安装的升级准备情况。获取更多信息，参考小节 2.11，升级 MySQL，特别是小节 2.11.5 为升级准备安装
