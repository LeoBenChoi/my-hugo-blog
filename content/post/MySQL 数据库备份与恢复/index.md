+++
date = '2025-11-12T13:10:09+08:00'
draft = false
title = 'MySQL 数据库备份与恢复'
image = "MySQL.png"
tags = [ 
	"MySQL", 
	"数据库管理",
] 
categories = [ 
	"数据库", 
]

+++

# MySQL 数据库备份与恢复

Mysql数据库冷备，使用 `mysqldump` 命令进行备份与恢复

## 备份

全库备份：

```sh
mysqldump -u root -p --all-databases > all_databases_backup.sql
```

备份单个数据库

```sh
mysqldump -u root -p database_name > database_backup.sql
```

备份特定表

```sh
mysqldump -u root -p database_name table_name > table_backup.sql
```

备份时增加压缩

```sh
mysqldump -u root -p database_name | gzip > database_backup.sql.gz
```

## 恢复

恢复全库

```sh
mysql -u root -p < all_databases_backup.sql
```

恢复单个数据库

```sh
mysql -u root -p database_name < database_backup.sql
```

恢复特定表

先在数据库中创建目标表，然后执行：

```sh
mysql -u root -p database_name < table_backup.sql
```

恢复压缩的备份

```sh
gunzip < database_backup.sql.gz | mysql -u root -p database_name
```
