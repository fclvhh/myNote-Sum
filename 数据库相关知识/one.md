# mongodb 快速入门

## 起步

> 创建两个命令行窗口



1. | 开启 **mongodb** 服务：要管理数据库，必须先开启服务，开启服务使用   **mongod --dbpath** |      |
   | ------------------------------------------------------------ | ---- |
   | **D:\mongoDB**                                               |      |

2. 管理 **mongoDB**
   数据库：mongo  (一定要在新的 cmd 中输入) 



```powershell
mongod --dbpath 文件目录路径
mongo 
```







## 和mysql的简单对比

![](D:\一“桶”前端\myNote\数据库相关知识\assets\mongodb.jpg)



mysql：

 		库 --> 表-->行-->字段

mongodb:

​		库--> 集合 -->文档 -->key:value(键值对)





# 数据库的CRUD操作

## 创建

MongoDB 提供 **insert** 方法创建新文档：

- `db.collection.inserOne()` 插入单个文档  
- `db.collection.inserMany()` 插入多个文档
- `db.collection.insert()` 插入单条或多条文档

```powershell
db.集合名.insert({name:sun,age:18})
```



## 查找（Read）

MongoDB 提供 ==**find** 方法==查找文档，第一个参数为查询条件：

