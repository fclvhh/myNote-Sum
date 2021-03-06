# DDL语言

> 核心: 创建模式 , 而不是模型

关键词 : create   alter  drop

模式的创建 , 修改 , 和删除



## 创建表

```sql
Create table `table表名`( `列名` 数据类型[Primary key |Unique][Not Null][,`列名`数据类型[Notnull] , ... ]) ;
```

1. “[ ] ”表示其括起的内容可以省略，“| ”表示其隔开的两项可取其一
2. Primary key:主键约束。每个表只能创建一个主键约束
3. Unique:唯一性约束(即候选键)。可以有多个唯一性约束。



> 小结 : 创建模式需要思考的点
>

1. 域: 字段的取值范围   就是数据类型
2. 元组的唯一标识  : 也就是确定候选键(Unique)/主键(Primary key)
3. 是否可以为空  null







# DML语言

> 核心: 表的相关操作: 增删改查

关键词 : Insert  Delete  Updata  Select



## 插入元组

```sql
insert into `表名`(`字段名`，...)
	values(值1，...);
```



## 改变元组

```sql
update 表名 set 字段=新值,字段=新值
【where 条件】
```







## Select元组查询

```sql
select `列名1`,`列名2`   from  `表1`,`表2`  where 检索条件
```

> Select查询操作的子句:

1. select 子句
2. from 子句
3. where 子句



> **单表查询:**

基本子句就可以完成

- select 子句  ==> 决定投影多少列
- where子句 ==>决定检索条件



> **功能增强:**

- 结果排序

  - 结果集根据哪一列 , 从小到大 还是 从大到小排列

  - ```sql
    order by `列名` asc or desco
    ```

  - order by 子句

- 模糊查询

  - 检索条件中添加 like
  - 匹配规则
    - %  匹配0个 or  多个
    - _ 匹配任意多个
    - \  转义字符
  - 举例
    - like 张%  : 所有姓张的
    - like 张_ _  : 张某某





### **多表联合查询:**

> 核心 , 多个表的检索

检索条件中包含连接条件







> 表更名

as





# DCL语言

> 核心: 权限控制

Grant  Revoke   :  授权和取消授权













# SQL 语言深入之复杂查询



## 子查询

> 定义:

出现在where 子句中的 select子句被称为子查询

子查询返回一个集合 , 可以通过与这个集合的比较来确定另一个查询集合

> 说人话:

1. 子查询会返回一个集合,这个集合就是一个关系s
2. 关系R 和 关系 S 进行连接查询
3. 实现了: **表与子表之间的联合查询**



> 应用场景

1. 集合成员资格判断
2. 集合间的比较
3. 集合基数的测试



**分析:**

1. 集合成员资格判断

举例: 

​	当域的值不是数字 , 而是其他含义的东西是

​	不能简单的大于/小于/等于  去限定检索范围

​	这个时候就需要用枚举

如: 

Sname in {张三,李四,王五}   就是一个判断条件

Sday in {周一 , 周二 , 周三, ...}   也就一个判断条件

非数字的集合 ,  数字式的关系判断就失效了



转而 用 是否在域中进行限定  in



限定集合 , 也是查询出来了 , 就是子查询



