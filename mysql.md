# mysql

## sql通用语法介绍

1. sql语句可以单行或多行书写，以分号结尾
2. sql语句可以使用空格/缩进来增强语句的可读性
3. mysql数据库的sql语句不区分大小写、关键字建议使用大写
4. 注释：
   1. 单行注释：# 注释内容
   2. 多行注释：/* 注释内容 */

## sql分类

1. DDL ：数据定义语言，用来定义数据库对象（数据库、表、字段）
2. DML ：数据操作语言，用来对数据表中的数据进行增删改查
3. DQL ：数据查询语言，用来查询数据库中表的记录
4. DCL ：数据控制语言，用来创建数据库用户，控制访问权限

## DDL

### 数据库操作

1. 查询
   
   1. 查询所有数据库：
      `show databases;`
   2. 查询当前数据库：
      `select database();`

2. 创建数据库
   `create database [if not exists] 数据库名 [default charset 字符集] [collate 排序规则];`
   ==注：== [...]中的内容可选

3. 删除数据库
   `drop database [if exists] 数据库名;`

4. 选择数据库
   `use 数据库名;`

### 表操作 --- 查询

1. 查询当前数据库所有表
   `show tables;`

2. 查询表结构
   `desc 表名;`

3. 查询指定表的建表语句
   `show create table 表名;`

### 表操作 --- 创建

```sql
create table 表名 (
    字段1 字段1类型 [comment '字段1注释'],
    字段2 字段2类型 [comment '字段1注释'],
    ...
) [comment '表注释'];

# 案例
create table user (
    id int primary key comment 'id',
    name varchar(50) comment 'name',
) comment 'user table';
```

### 表操作 --- 数据类型

> mysql数据类型主要分为三类：`数值类型`、`字符串类型`、`日期时间类型`

#### 数值类型

| 类型          | 大小     | 有符号范围       | 无符号范围    | 描述         |
| ----------- | ------ | ----------- | -------- | ---------- |
| tinyint     | 1bytes | (-128, 127) | (0, 255) | 小整数        |
| smallint    | 2bytes | ()          | ()       | 较大整数       |
| mediumint   | 3bytes | ()          | ()       | 中等大小整数     |
| int或integer | 4bytes | ()          | ()       | 大整数        |
| bigint      | 8bytes | ()          | ()       | 极大整数       |
| float       | 4bytes | ()          | ()       | 单精度浮点数     |
| double      | 8bytes | ()          | ()       | 双精度浮点数     |
| decimal     | --     | ()          | ()       | 小数值(精确定点数) |

#### 字符串类型

| 类型         | 大小                  | 描述             |
| ---------- | ------------------- | -------------- |
| char       | (0-255)bytes        | 定长字符串          |
| varchar    | (0-65535)bytes      | 变长字符串          |
| tinytext   | (0-255)bytes        | 短文本字符串         |
| blob       | (0-65535)bytes      | 二进制形式的长文本数据    |
| text       | (0-65535)bytes      | 长文本数据          |
| mediumblob | (0-16777215)bytes   | 二进制形式的中等长度文本数据 |
| mediumtext | (0-16777215)bytes   | 中等长度文本数据       |
| longblob   | (0-4294967295)bytes | 二进制形式的极大文本数据   |
| longtext   | (0-4294967295)bytes | 极大文本数据         |

#### 时间类型

| 类型        | 大小     | 范围                                      | 格式           | 描述       |
| --------- | ------ | --------------------------------------- | ------------ | -------- |
| date      | 3bytes | 1000-01-01至9999-12-31                   | 格式YYYY-MM-DD | 日期值      |
| time      | 3bytes | -838:59:59至838:59:59                    | HH:MM:SS     | 时间值或持续时间 |
| year      | 1bytes | 1901至2155                               | YYYY         | 年份值      |
| datetime  | 8bytes | 1000-01-01 00:00:00至9999-12-31 23:59:59 | --           | 混合日期和时间  |
| timestamp | 4bytes | 1970-01-01 00:00:01至2038-01-19 03:14:07 | --           | 时间戳      |

### 表管理 --- 结构修改

1. 给表添加新的字段
   `alter table 表名 add 字段名 类型 [comment 注释] [约束];`

2. 修改数据类型
   `alter table 表名 modify 字段名 新数据类型;`

3. 修改字段名和字段类型
   `alter table 表名 change 旧字段名 新字段名 类型 [comment 注释] [约束];`

4. 删除字段
   `alter table 表名 drop 字段名;`

5. 修改表名
   `alter table 表名 rename to 新表名;`

6. 删除表
   `drop table [if exists] 表名;`

7. 删除指定表，并重新创建该表
   `truncate table 表名;`

## DML

> 用来对数据库中表的数据记录进行增删改查

### 数据管理 --- 添加数据

1. 给指定字段添加数据
   
   ```sql
   insert into 表名 (字段1, 字段2, ...) values (值1, 值2, ...);
   ```

2. 给全部字段添加数据
   
   ```sql
   insert into 表名 values (值1, 值2, ...);
   ```

3. 批量添加数据
   `insert into 表数据 (字段1, 字段2, ...) values (值1, 值2, ...), (值1, 值2, ...), ...;`
   或
   `insert into 表名 values (值1, 值2, ...), (值1, 值2, ...), ...;`

<mark>注：</mark>

- 插入数据时，字段名顺序要与值的顺序一一对应
- 字符串和日期型数据应该包在引号中
- 插入的数据大小应该在字段的规定范围内

### 数据管理 --- 修改数据

`update 表名 set 字段名1=值1, 字段名2=值2, ... [where 条件];`

<mark>注：</mark> 如果没有条件语句，则会修改整张表对应字段的数据
例：

```sql
update emp set idcard=20 where id=3;
```

### 数据管理 --- 删除数据

`delete from 表名 [where 条件];`
<mark>注：</mark> 如果没有条件语句，会删除整张表

## DQL

### 表数据查询 --- 基本查询

1. 查询多个字段
   `select 字段1, 字段2, ... from 表名;`
   `select * from 表名;`

2. 设置别名
    `select 字段1 as 别名1, 字段2 as 别名2, ... from 表名;`

3. 去除重复记录
   `select distinct 字段列表 from 表名;`

### 表数据查询 --- 条件查询

1. 语法
   `select 字段列表 from 表名 where 条件列表;`

2. 条件列表
   在条件列表中可以使用 >、 >=、 <、 <=、 =、 <>(!=)
   between ... and ... (在某个范围之内包含边界)
   in(...) 在列表中的值，多选一
   like 匹配字符串 模糊匹配(_匹配单个字符，%匹配任意个字符)
   is null 为null

### 聚合函数

> 将一列数据作为一个整体进行纵向计算

1. 常见聚合函数
   
   | 函数    | 功能   |
   | ----- | ---- |
   | count | 统计数量 |
   | max   | 最大值  |
   | min   | 最小值  |
   | avg   | 平均值  |
   | sum   | 求和   |

2. 案例
   `select sum(socre) from student;`

### 分组查询

1. 语法
   `select 字段列表 from 表名 [where 条件] group by 分组字段名 [having 分组的过滤条件];`

2. where 与 having 的区别
   
   1. 执行时机不同：where是分组之前进行过滤，不满足条件不参与分组，而having是分组之后对结果进行过滤
   2. 判断条件不同：where不能对聚合函数进行判断，而having可以

<mark>注：</mark>

- 执行顺序：where > 聚合函数 > having
- 分组之后：查询的字段一般为聚合函数和分组字段，即select后的字段列表应为分组字段或聚合函数

### 排序查询

1. 语法
   `select 字段列表 from 表名 order by 字段1 排序1, 字段2 排序2,...;`

2. 排序方式
   asc：升序（默认值）
   desc：降序
   <mark>注：</mark> 如果多个字段排序当第一个字段值相同时，才会根据第二个字段进行排序

### 限制查询行数（可以用于分页查询）

> 限制输出查询的行数

1. 语法
   `select 字段列表 from 表名 limit 起始索引, 查询记录数;`
    <mark>注：分页查询</mark>
   
   - 起始索引从0开始，起始索引=(查询页码 - 1) * 每页显示记录数
   - 分页查询是数据库的方言，不同的数据库有不同的实现，这里是mysql实现
   - 如果查询的是第一页数据，起始索引可以省略

### 执行顺序

1. `FROM / JOIN`  (加载数据)
   
          ↓

2. `WHERE`        (过滤原始行)
   
          ↓

3. `GROUP BY`     (分组)
   
          ↓

4. `HAVING`      (过滤分组)
   
          ↓

5. `SELECT`       (选择列/计算)
   
          ↓

6. `DISTINCT`     (去重)
   
          ↓

7. `ORDER BY`     (排序)
   
          ↓

8. `LIMIT`        (截取)

## DCL

> DCL是用来管理数据库用户，控制数据库的访问权限

### 管理用户

1. 查询用户
   
   ```sql
   use mysql;
   select * from user;
   ```

2. 创建用户
   `create user '用户名'@'主机名' identified by '密码';`

3. 修改用户密码
   `alter user '用户名'@'主机名' identified with mysql_native_password by '新密码';`

4. 删除用户名
   `drop user '用户名'@'主机名';`

==注：== 主机名可以使用`%`通配

### 权限控制

| 常用权限   | 说明         |
| ------ | ---------- |
| all    | 所有权限       |
| select | 查询数据       |
| insert | 插入数据       |
| update | 修改数据       |
| delete | 删除数据       |
| alter  | 修改表        |
| drop   | 删除数据库/表/视图 |
| create | 创建数据库/表    |

1. 查询用户的权限
   `show grants for '用户名'@'主机名';`

2. 授予权限
   `grants 权限列表 on 数据库.表名 from '用户名'@'主机名';`

3. 撤销权限
   `remove 权限列表 on 数据库.表名 from '用户名'@'主机名';`

<mark>注：</mark>

- 多个权限之间，使用逗号分隔
- 授权时，数据库名和表名可以使用*进行通配，代表所有

## 函数

### 字符串函数

| 常用函数                     | 功能                            |
| ------------------------ | ----------------------------- |
| concat(s1, s2,...)       | 字符串拼接，将s1，s2...拼接成一个字符串       |
| lower(str)               | 将字符串str全部转成小写                 |
| upper(str)               | 将字符串str全部转成大写                 |
| lpad(str,n,pad)          | 左填充，用字符串pad对str进行填充，达到n个字符串长度 |
| rpad(str,n,pad)          | 右填充，用字符串pad对str进行填充，达到n个字符串长度 |
| trim(str)                | 去掉字符串头部和尾部的空格                 |
| substring(str,start,len) | 返回字符串str从start位置起的len个长度的字符串  |

### 数值函数

| 常用函数       | 功能                 |
| ---------- | ------------------ |
| ceil(x)    | 向上取整               |
| floor(x)   | 向下取整               |
| mod(x)     | 返回x/y的模            |
| rand()     | 返回0~1内的随机值         |
| round(x,y) | 求参数x的四舍五入的值，保留y位小数 |

### 日期函数

| 常用函数                              | 功能                         |
| --------------------------------- | -------------------------- |
| curdate()                         | 返回当前日期                     |
| curtime()                         | 返回当前时间                     |
| now()                             | 返回当前日期和时间                  |
| year(date)                        | 获取指定date的年份                |
| month(date)                       | 获取指定date的月份                |
| day(date)                         | 获取指定date的日期                |
| date_add(date,interval expr type) | 返回一个日期/时间值加上一个时间间隔expr的时间值 |
| datediff(date1, date2)            | 返回起始时间date1和结束时间date2之间的天数 |

### 流程函数

可以在sql语句中实现条件筛选，从而提高语句的效率

| 函数                                                    | 功能                                  |
| ----------------------------------------------------- | ----------------------------------- |
| if(value,t,f)                                         | 如果value为true，则返回t，否则返回f             |
| ifnull(value1, value2)                                | 如果value1不为空，返回value1，否则返回value2     |
| case when value1 then res1... else default end        | 如果value1为true返回res1，否则返回default默认值  |
| case [expr] when [v1] then [r1]... else [default] end | 如果expr的值等于v1，返回r1... 否则返回default默认值 |

## 约束

> 定义：约束是作用于表中字段上的规则，用于限制存储在表中的数据

> 目的：保证数据库中数据的正确性、有效性和完整性

分类：

| 约束   | 描述                           | 关键字         |
| ---- | ---------------------------- | ----------- |
| 非空约束 | 限制该字段的数据不能为null              | not null    |
| 唯一约束 | 保证该字段的所有数据都是唯一的              | unique      |
| 主键约束 | 主键是一行数据的唯一标识，要求非空且唯一         | primary key |
| 默认约束 | 保存数据时，如果未指定数据则采用默认值          | default     |
| 检测约束 | 保证字段值满足某一个条件                 | check       |
| 外键约束 | 用来让两张表的数据之间建立连接，保证数据的一致性和完整性 | foreign key |

### 外键约束

> 定义：外键用来让两张表的数据之间建立连接，从而保证数据的一致性和完整性

1. 创建外键语法：
   
   ```sql
   # 创建主表（被引用的表）
   create table 主表名 (
    id int primary key auto_increment;
    mname varchar(50) not null;
   )
   # 创建子表（包含外键的表）
   create table 子表名 (
    字段名 数据类型,
    main_table_id int,
   
    # 定义外键，以上面被引用的表为例
    # fk_child_table： 外键名称
    # main_table_id： 子表列名 用来绑定主表对应列
    # 主表(id)： 主表名(主表列名)
    constraint fk_child_table foreign key (main_table_id) references 主表(id) [on delete 行为] [on update 行为]
   )
   
   ```

或者

```sql
alter table 表名 add constraint 外键名称 foreign key (外键字段名) references 主表(主表列表名);
```

2. 删除外键
   `alter table 表名 drop foreign key 外键名称;`

3. 删除/更新行为
   
   | 行为        | 说明  |
   | --------- | --- |
   | no active | 当在  |
   |           |     |
   
   
   
   
   
   

## 索引

### 索引概述

> 索引（index）是帮助MySQL高效获取数据的<mark>数据结构</mark>（<mark>有序</mark>）。在数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据，这样就可以在这些数据结构上实现高级查找算法，这种数据结构就是索引。

**优缺点**

| 优势                              | 劣势                                                             |
| ------------------------------- | -------------------------------------------------------------- |
| 提高数据检索的效率，降低数据库的IO成本            | 索引列也是要占用空间的                                                    |
| 通过索引列对数据进行排序，降低数据排序的成本，降低CPU的消耗 | 索引大大提高了查询效率，同时却也降低更新表的速度，如对表进行`insert`、`update`、`delete`时，效率降低 |

### 索引结构

#### 索引结构介绍

**MySQL的索引是在存储引擎层实现的，不同的存储引擎有不同的结构，主要包含以下几种：**

| 索引结构            | 描述                                         |
| --------------- | ------------------------------------------ |
| B+树索引           | 最常见的索引类型，大部分引擎都支持B+树索引                     |
| Hash索引          | 底层数据结构是用哈希表实现的，只有精确匹配索引列的查询才有效，不支持范围查询     |
| R-树（空间索引）       | 空间索引是MyISAM引擎的一个特殊索引类型，主要用于地理空间数据类型，通常使用较少 |
| Full-text（全文索引） | 是一种通过建立倒排索引，快速匹配文档的方式。类似于es                |

##### 引擎支持情况

| 索引        | InnDB    | MyISAM | Memory |
|:---------:|:--------:|:------:|:------:|
| B+树索引     | √        | √      | √      |
| Hash索引    | ×        | ×      | √      |
| R-树索引     | ×        | √      | ×      |
| Full-text | 5.6版本后支持 | √      | ×      |

注：如果没有特别指明，索引一般指的是B+树索引

#### B树索引结构

##### 二叉树

> 二叉树缺点：顺序插入时，会形成一个链表，查询性能大大降低。大量数据情况下，层级较深，检索速度慢。

<img title="" src="./pic/mysql/屏幕截图 2026-04-19 122950.png" alt="">

> 解决顺序链表问题可以使用红黑树解决，但是仍然存在大数据量情况下，层级较深，检索速度慢。

<img title="" src="./pic/mysql/屏幕截图 2026-04-19 123225.png" alt="">

##### B树（多路平衡查找树）

> 树的度数指的是一个节点的子节点个数

<img title="" src="./pic/mysql/屏幕截图 2026-04-19 123646.png" alt="">

<mark>注：</mark>可以找一个动画看一下B树插入数据的过程

##### B+树

> B+树的数据都存放在叶子节点上，非叶子节点只起到索引作用，叶子节点都连接起来形成一个链表

<img title="" src="./pic/mysql/屏幕截图 2026-04-19 124123.png" alt="">

##### MySQL的B+树

> MySQL索引数据结构对经典的B+树进行了优化。在原B+树的基础上，增加一个指向相邻叶子节点的链表指针，就形成了带有顺序指针的B+树，提高区间访问的性能。

<img title="" src="./pic/mysql/屏幕截图 2026-04-19 124433.png" alt="">

##### Hash

> 哈希索引就是采用一定的hash算法，将键值换算成新的hash值，映射到对应的槽位上，然后存储在hash表中。
> 
> 如果两个（或多个）键值映射到一个相同的槽位上，它们就产生了hash冲突，可以通过链表来解决。

<img title="" src="./pic/mysql/屏幕截图 2026-04-19 125041.png" alt="">

> hash索引的特点：
> 
> - hash索引只能用于对等比较（=，in）不支持范围查询（> ，< ）
> 
> - 无法利用索引完成排序操作
> 
> - 查询效率高，通过只需要一次检索就可以了，效率通过要高于B+树索引

#### 思考

1. 为什么InnoDB存储引擎选择使用B+树索引结构
   
   1. 相较于二叉树，层级更少，搜索效率高
   
   2. 对于B树，无论叶子节点还是非叶子节点，都会保存数据，这样导致一页中存储的键值对减少，指针跟着减少，要同样保存大量数据，只能增加树的高度，导致性能降低
   
   3. 相较于hash索引，B+树支持范围索引

### 索引分类

| 分类   | 含义                          | 特定           | 关键字      |
| ---- | --------------------------- | ------------ | -------- |
| 主键索引 | 针对于表中主键创建的索引                | 默认自动创建，只能有一个 | primary  |
| 唯一索引 | 避免同一个表中某数据列中的值重复            | 可以有多个        | unique   |
| 常规索引 | 快速定位特定数据                    | 可以有多个        |          |
| 全文索引 | 全文索引查找的就是文本中的关键词，而不是比较索引中的值 | 可以有多个        | fulltext |

**在InnoDB存储引擎中，根据索引的存储形式，又可以分为以下两种：**

| 分类   | 含义                            | 特点         |
| ---- | ----------------------------- | ---------- |
| 聚集索引 | 将数据存储与索引放到了一块，索引结构的叶子节点保存了行数据 | 必须有，而且只有一个 |
| 二级索引 | 将数据与索引分成存储，索引结构的叶子节点关联的是对应的主键 | 可以存在多个     |

> 聚集索引选取规则：
> 
> - 如果存在主键，主键索引就是聚集索引
> 
> - 如果不存在主键，将使用第一个唯一(unique)索引作为聚集索引
> 
> - 如果表没有主键也没有合适的唯一索引，则InnoDB会自动生成一个rowid作为隐藏的聚集索引

<img title="" src="./pic/mysql/屏幕截图 2026-04-19 131221.png" alt="">

> select * from xx where name="arm";
> 
> 上述sql搜索流程是：
> 
> - 先去二级索引查找，找到id，再根据id对聚集索引查找全部的数据行

### 索引语法

1. 创建索引
   
   ```sql
   // unique | fulltext 是可选项，表明索引类型
   // unique：表明创建的是一个唯一索引
   // fulltext：全文索引
   // index_name：要创建索引的名字
   // table_name：在哪个表上创建索引
   // index_col_name：字段名，如果有多个字段名说明是一个联合索引
   create [unique | fulltext] index index_name on table_name (index_col_name, ...);
   ```

2. 查看索引
   
   ```sql
   show index from table_name;
   ```

3. 删除索引
   
   ```sql
   drop index index_name on table_name;
   ```

### 索引性能分析

#### SQL性能分析

- SQL执行频率
  
  > MySQL客户端连接成功后，通过`show [session | global] status`命令可以提供服务器状态信息。通过如下指令，可以查看当前数据库的insert、update、delete、select的访问频次：
  
  ```sql
  # Com后面是7个下划线
  show global status like 'Com_______';
  # 输出示例
  # +---------------+-------+
  # | Variable_name | Value |
  # +---------------+-------+
  # | Com_binlog    | 0     |
  # | Com_commit    | 0     |
  # | Com_delete    | 0     | 删除次数
  # | Com_import    | 0     |
  # | Com_insert    | 1     | 插入次数
  # | Com_repair    | 0     |
  # | Com_revoke    | 0     |
  # | Com_select    | 17    | 查询次数
  # | Com_signal    | 0     |
  # | Com_update    | 0     | 更新次数
  # | Com_xa_end    | 0     |
  # +---------------+-------+
  # 11 rows in set (0.01 sec)
  ```

- 慢查询日志
  
  > 慢查询日志记录了所有执行时间超过指定参数（long_query_time，单位：秒，默认10秒）的所有sql语句日志。MySQL的慢查询日志默认没有开启，需要在MySQL的配置文件（/etc/my.cnf）中配置如下信息：
  
  ```sql
  # 查看是否开启慢查询日志
  show variables like 'slow_query_log';
  # 开启mysql的慢查询日志
  slow_query_log=1
  # 设置慢日志的时间为2秒，sql语句执行时间超过2秒，就会视为慢查询，记录慢查询日志
  long_query_time=2
  ```

- profile详情
  
  > 慢查询只记录超过一定时间的查询，对某些简单的查询它们可能没用超过设定的时间但是也很耗费时间，我要了解这些查询可以使用profile。
  > show profiles能够在做sql优化时帮助我们了解时间都耗费到哪里去了。通过have_profiling参数，能够看到当前MySQL是否支持profile操作：
  
  ```sql
  # 查看是否支持profile
  select @@have_profiling;
  # 查看profile是否开启
  select @@profiling;
  # 默认profiling是关闭的，可以通过set语句在 session/global级别开启profiling:
  set profiling=1;
  
  # 查看每一个命令执行时间
  show profiles;
  # 输出示例
  # +----------+------------+--------------------------------------+
  # | Query_ID | Duration   | Query                                |
  # +----------+------------+--------------------------------------+
  # |        1 | 0.00077400 | select @@profiling                   |
  # |        2 | 0.00048275 | select count(*) from user            |
  # |        3 | 0.00045650 | SELECT DATABASE()                    |
  # |        4 | 0.00775775 | select count(*) from user            |
  # |        5 | 0.00022950 | select @@profiling                   |
  # |        6 | 0.00273975 | show variables like 'slow_query_log' |
  # +----------+------------+--------------------------------------+
  # 6 rows in set, 1 warning (0.00 sec)
  
  # 查看指定query_id的sql语句各个阶段耗时情况
  show profile for query 4;
  # 输出示例
  # +--------------------------------+----------+
  # | Status                         | Duration |
  # +--------------------------------+----------+
  # | starting                       | 0.000070 |
  # | Executing hook on transaction  | 0.000004 |
  # | starting                       | 0.000009 |
  # | checking permissions           | 0.000005 |
  # | Opening tables                 | 0.005440 |
  # | init                           | 0.000020 |
  # | System lock                    | 0.000011 |
  # | optimizing                     | 0.000041 |
  # | statistics                     | 0.000029 |
  # | preparing                      | 0.000149 |
  # | executing                      | 0.001799 |
  # | end                            | 0.000010 |
  # | query end                      | 0.000006 |
  # | waiting for handler commit     | 0.000014 |
  # | closing tables                 | 0.000009 |
  # | freeing items                  | 0.000128 |
  # | cleaning up                    | 0.000017 |
  # +--------------------------------+----------+
  # 17 rows in set, 1 warning (0.00 sec)
  ```

- explain执行计划
  
  > explain或者desc命令获取MySQL如何执行select语句的信息，包括在select语句执行过程中表如何连接和连接的顺序。
  > 使用如下：
  
  ```sql
  # 直接在select语句之前加上关键字explain
  explain select 字段列表 from 表名 where 条件;
  # 新版mysql的输出与旧版mysql不一致，可以进行如下设置
  SET explain_format = 'TRADITIONAL';
  
  explain select * from user where profession='销售';
  # 输出
  # +----+-------------+-------+------------+------+----------------------+----------------------+---------+-------+------+----------+-------+
  # | id | select_type | table | partitions | type | possible_keys        | key                  | key_len | ref   | rows | filtered | Extra |
  # +----+-------------+-------+------------+------+----------------------+----------------------+---------+-------+------+----------+-------+
  # |  1 | SIMPLE      | user  | NULL       | ref  | idx_user_pro_age_sta | idx_user_pro_age_sta | 203     | const |   40 |   100.00 | NULL  |
  # +----+-------------+-------+------------+------+----------------------+----------------------+---------+-------+------+----------+-------+
  # 1 row in set, 1 warning (0.00 sec)
  ```

         **explain执行计划各个字段含义：** 

| 字段名          | 含义                                                                           | 重要性 |
| ------------ | ---------------------------------------------------------------------------- | --- |
| id           | select查询的序列号，表示查询中执行select子句或者操作表的顺序（id相同，执行顺序从上到下；id不同，值越大，越先执行）            |     |
| select_type  | 表示select的类型，常见的取值有simple（简单表，即不使用表连接或子查询）、primary（主查询，即外层的查询）、union、subquery |     |
| type         | 表示连接类型，性能由好到差的连接类型为null、system、const、eq_ref、ref、range、index、all。             | 重要  |
| possible_key | 显示可能应用在这张表上的索引，一个或多个                                                         | 重要  |
| key          | 实际使用的索引，如果为null，则没用使用索引                                                      | 重要  |
| key_len      | 表示索引中使用的字节数，该值为索引字段最大可能长度，并非实际使用长度，在不损失精确性的前提下，长度越短越好。                       | 重要  |
| rows         | mysql认为必须要执行查询的行数，在innodb引擎的表中，是一个估计值，可能不是准确的                                |     |
| filtered     | 表示返回结果的行数占需读取行数的百分比，filtered的值越大越好                                           |     |
| extra        | 额外信息                                                                         | 重要  |

### 索引的使用规则

#### 最左前缀法则

> 如果索引了多列（联合索引），要遵守最左前缀法则。最左前缀法则指的是查询从索引的最左列开始，并且不跳过索引中的列。如果跳过某一列，<mark>索引将部分失效（后面的字段索引失效）</mark>

**案例**

```sql
# 创建了如下联合索引
create index index_user_pro_age_sta on t_user (profession, age, status);

# 使用如下查询，走上面的联合索引
select * from t_user where profession='软件工程';

# 走上面的联合索引
select * from t_user where profession='软件工程' and age=20;

# 不走上面的联合索引，因为最左边的列profession不存在，后面的age status字段索引失效
select * from t_user where age=20;

# 部分走索引，profession字段走索引，而status字段不走索引，因为age字段不存在，所以age字段右侧的字段不走索引
select * from t_user where profession='软件工程' and status=1;

# 全部走索引，因为最左前缀法则只要求字段存在，不要求字段在sql中的顺序
select * from t_user where status=1 and age=20 and profession='软件工程';
```

#### 范围查询

> 联合索引中，出现范围查询（>,<），<mark>范围查询右侧的列索引失效</mark>

**案例**

```sql
# 创建了如下联合索引
create index index_user_pro_age_sta on t_user (profession, age, status);

# 使用如下索引，status不会走索引
select * from t_user where profession='软件工程' and age>20 and status='0';

# 避免索引失效，带等号，下面的status索引不会失效
select * from t_user where profession='软件工程' and age>=20 and status='0';
```

#### 索引列运算

> 不要再索引列上进行运算操作，<mark>否则索引将失效</mark>。

**案例**

```sql
# 创建了如下联合索引
create index index_user_phone on t_user phone;

# 下面的索引失效
select * from t_user where substring(phone,10,2)='15';
```

#### 字符串引号

> 字符串类型字段使用时，不加引号，索引将失效。

```sql
# 创建了如下联合索引
create index index_user_phone on t_user phone;

# phone是字符串类型，不加引号导致索引失效
select * from t_user where phone=15111111;
```

#### 模糊查询

> 如果仅仅是尾部模糊匹配，索引不会失效。如果是头部模糊匹配，索引失效。

```sql
# 创建了如下联合索引
create index index_user_phone on t_user phone;

# 索引失效
select * from t_user where phone='%15';
# 索引不会失效
select * from t_user where phone='15%';
# 索引失效
select * from t_user where phone='%15%';
```

#### or连接的条件

> 用or分隔开的条件，如果or前的条件中的列有索引，而后面的列中没有索引，那么涉及的索引都不会被用到。

```sql
# age字段没有索引

# 下面的主键索引会失效，因为age没有索引
# 解决：给age添加索引
select * from t_user where id=23 or age=30;
```

#### 数据分布影响

> 如果MySQL评估使用索引比全表扫描更慢，则不使用索引。
> 
> 案例：如果一个sql的结果几乎是整张表，那么不如直接全表扫描更比。

#### SQL提示

> SQL提示，是优化数据库的一个重要手段，简单来说，就是在SQL语句中加入一些人为的提示来达到优化操作的目的。
> 
> 主要就是当一个字段有多个不同的索引时，提示MySQL选择使用哪个索引

```sql
# 提示mysql选择使用索引idx_user_pro，mysql不一定使用这个索引
select * from t_user use index(idx_user_pro) where profession='软件工程';

# 提示mysql忽略索引idx_user_pro
select * from t_user ignore index(idx_user_pro) where profession='软件工程';

# 提示mysql必须使用索引idx_user_pro
select * from t_user force index(idx_user_pro) where profession='软件工程';
```

#### 覆盖索引

> 尽量使用覆盖索引（查询使用了索引，并且需要返回的列，在该索引中已经全部能够找到），减少`select *`。
> 
> 索引得到的结果是索引字段的值和所在行的主键id，如果要查询字段不是被使用索引的字段，那么mysql需要拿id再去聚集索引（主键索引）得到行数据，再去拿到到目标字段值。
> 
> 这种拿id再去聚集索引查找，叫回表查询

<img title="" src="./pic/mysql/屏幕截图 2026-04-21 105300.png" alt="">

#### 前缀索引

> 单字段类型为字符串（varchar，text等）时，有时候需要索引很长的字符串，这会让索引变得很大，查询时，浪费大量的磁盘IO，影响查询效率。此时可以只将字符串的一部分前缀，建立索引，这样可以大大节约索引空间，从而提高索引效率。

```sql
# 创建前缀索引
create index idx_xx on table_name(column(n));
```

> 前缀长度：
> 
>     可以根据索引的选择性来决定，而选择性是指不重复的索引值（基数）和数据表的记录总数的比值，索引选择性越高则查询效率越高，唯一索引的选择性是1，这是最好的索引选择性，性能也是最好的。（其实就是区分度，看看差异大不大。）

```sql
# 计算选项性
select count(distinct email)/count(*) from t_user;

# 计算前缀的选择性
select count(distinct substring(email,1,5))/count(*) from t_user;
```

#### 单列索引与联合索引

> - 单列索引：即一个索引只包含单个列。
> 
> - 联合索引：即一个索引包含了多个列。
> 
> 在业务场景中，如果存在多个查询条件，考虑针对查询字段建立索引时，建议建立联合索引，而非单列索引。

**联合索引情况**

<img title="" src="./pic/mysql/屏幕截图 2026-04-21 115019.png" alt="">

#### 索引的设计原则

> 1. 针对于数据量较大，且查询比较频繁的表建立索引
> 
> 2. 针对于常作为查询条件（where）、排序（order by）、分组（group by）操作的字段建立索引
> 
> 3. 尽量选择区分度高的列作为索引，尽量建立唯一索引，区分度越高，使用索引的效率越高
> 
> 4. 如果是字符串类型的字段，字段的长度较长，可以针对于字段的特点，建立前缀索引
> 
> 5. 尽量使用联合索引，减少单列索引，查询时，联合索引很多时候可以覆盖索引，避免回表
> 
> 6. 要控制索引的数量，索引越多，维护索引的结构的代价也就越大，会影响增删改的效率
> 
> 7. 如果索引列不能存储null值，在建表时使用not null约束

## SQL优化

### 插入数据优化

- insert优化
  
  - 批量插入
  
  ```sql
  insert into t_user values(1, 't'), (2, 'm');
  ```
  
  - 手动提交事务
  
  ```sql
  start transaction;
  insert into t_user values(1, 't'), (2, 'm');
  insert into t_user values(3, 't'), (4, 'm');
  commit;
  ```
  
  - 主键顺序插入

- 大批量插入数据
  
  > 如果一次性需要插入大批量数据，使用insert语句插入性能较低，此时可以使用MySQL数据库提供的load指令进行插入。
  > 使用步骤如下：
  
  ```sql
  # mysql客户端连接服务器时，加上参数 --local-infile
  mysql --local-infile -u root -p
  
  # 设置全局参数local_infile为1，开启从本地加载文件导入数据的开关
  set global local_infile=1;
  
  # 执行load指令
  # 加载文件 /root/sql1.log
  # 文件内容格式：
  #    1,d1,17867379089,23
  #    2,d2,17867379010,24
  # 插入表 tb_user
  # 字段分隔符 ,
  # 行分隔符 \n
  load data local infile '/root/sql1.log' into table `tb_user` fields terminated by ',' lines terminated by '\n';
  ```

        <mark>主键顺序插入性能高于乱序插入</mark>

### 主键优化

- 数据组织方式
  
  > 在InnoDB存储引擎中，表数据都是根据主键顺序组织存放的，这种存储方式的表称为索引组织表（index organized table IOT）。

- 页分裂

- 页合并

- 主键设计原则
  
  - 满足业务需求的情况下，尽量降低主键的长度
  
  - 插入数据时，尽量选择顺序插入
  
  - 尽量不要使用uuid或者其它自然做主键做主键

### order by优化

MySQL中的排序有两种方式：

1. Using filesort：通过表的索引或全表扫描，读取满足条件的数据行，然后在排序缓冲区sort buffer中完成排序操作，所有不是通过索引直接返回排序结果的排序都叫FileSort排序。

2. Using index：通过有序索引排序扫描直接返回有序数据，这种情况即为using index，不需要额外排序，操作效率高。（就是把要排序的字段建立一个索引）

```sql
# 创建索引
create index idx_user_phone_age on t_user(age,phone);

# 根据age，phone进行升序排序
# using index
select id, age, phone from t_user order by age, phone;

# 根据age，phone进行降序排序
# using index
select id, age, phone from t_user order by age desc, phone desc;

# 根据age升序，phone降序
# using filesort，性能差
select id, age, phone from t_user order by age asc, phone desc;
# 解决：建立对应索引
create index idx_user_phone_age on t_user(age asc,phone desc);
```

> - 根据排序字段建立合适的索引，多字段排序时，也遵循最左前缀法则
> 
> - 尽量使用覆盖索引
> 
> - 多字段排序，一个升序一个降序，此时需要注意联合索引在创建时的规则（asc/desc）
> 
> - 如果不可避免的出现filesort，大数据量排序时，可以适当增大排序缓冲区大小sort_buffer_size

### group by优化

> 给分组字段建立索引
> 
> 分组操作时，索引的使用也是满足最左前缀法则

### limit优化

> 一个常见又非常头疼的问题就是limit 2000000,10，此时需要MySQL排序前20000010记录，仅仅返回2000000 - 2000010的记录，其它记录丢弃，查询排序的代价非常大。
> 
> 优化思路，一般分页查询时，通过创建覆盖索引能够较好的提高性能，可以通过覆盖索引加子查询形式进行优化。

### count优化

> - MyISAM引擎把一个表的总行数存在了磁盘上，因此执行count(*)的时候会直接返回这个数，效率很高
> 
> - InnoDB引擎就很麻烦了，它执行count(*)的时候，需要把数据一行一行的从引擎里面读取出来，然后累计计数。
> 
> 优化思路：自己计数

<mark>按照效率排序：count(字段) < count(主键id) < count(1) ≈ count(*)</mark>

### update优化

> <mark>InnoDB的行锁是针对索引加的锁，不是针对记录加的锁，并且该索引不能失效，否则会从行锁升级为表锁。</mark>
> 
> 优化思路：也就是把要更新的字段添加索引

## 视图

> 介绍：视图（view）是一种虚拟存在的表。视图中的数据并不在数据库中实际存在，行和列数据来自定义视图的查询中使用的表，并且是在使用视图时动态生成的。
