# Mysql文档

---

[toc]

---



## 1. Mysql简介

---

### 1.1 Mysql简介

Mysql是一个关系型数据库管理系统

由C++语言编写。

开源

体积小、速度快、总体拥有成本低、招人成本低

中小型网站、或者大型网站、集群

---

### 1.2 数据库分类

#### 1.2.1 关系型数据库：（SQL）

+ MYSQL，Oracle，Sql Server，DB2，SQLlite
+ 通过表和表之间，行和行之间的关系进行数据的存储     `学员信息表，考勤表`

#### 1.2.2 非关系型数据库：（NoSQL）

+ Redis，MongDB
+ 通过对象自身的属性来决定对象存储
+ --

### 1.3 数据库管理系统

---

(DBMS)







---

## 2. Mysql的安装配置和目录结构

---

### 2.1 Mysql安装

Mysql官网：[MySQL](https://www.mysql.com/)

Mysql版本：5.7

---

### 2.2 Mysql配置

#### 2.2.1 环境变量配置

+ 在系统环境变量中，添加：
  + MYSQL_HOME     mysql目录下的bin目录
+ 在系统环境变量的path中，添加：
  + %MYSQL_HOME%

#### 2.2.2 配置文件配置

在mysql的bin文件夹所在的mysql根目录下新建my.ini文件如下图

```ini
[mysqld]

#设置3306端

port = 3306

# 设置mysql的安装目录

basedir=D:\mysql57

# 设置mysql数据库的数据的存放目录

datadir=D:\mysql57\data

# 允许最大连接数

max_connections=200

# 服务端使用的字符集默认为8比特编码的latin1字符集

character-set-server=utf8

# 创建新表时将使用的默认存储引擎

default-storage-engine=INNODB

sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES

[mysql]

# 设置mysql客户端默认字符集

default-character-set=utf8

```

---

### 2.3 Mysql插件

#### 2.3.1 Mysql命令行补全

pip下载

`pip install mycliCollecting mycli`

更新: 最新版Win10有bug

---

### 2.4 Mysql文件格式

InnoDB是MySQL默认的存储引擎，也是MySQL使用最广泛的存储引擎，InnoDB存储数据的物理文件通常以ibd作为其文件名后缀，本文将结合源码，简单介绍ibd文件的整体结构。

innodb ibd文件以页为单位进行管理，默认情况下页大小为16k，ibd文件的大小必然为16k的整数倍。页的结构整体上可以分为页头、页身、页尾。其中页头占用固定的38字节，页尾占用固定的8字节，其余都为页身，ibd文件的每个页无一例外，都是这样的结构

ibd文件:表数据存储文件
frm文件:表结构文件
opt文件:记录该数据库的默认字符集编码和字符集排序规则                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   











## 3. Mysql使用步骤

---

管理员模式下打开cmd

---

### 3.1 进入bin文件夹

先在命令行中进入指定盘符：

`d:`

进入D盘

`cd D:\Environment\mysql-5.7.35\bin`

cd进入 MySQL文件夹下bin文件夹 

### 3.2 初始化服务器

---

`mysqld --install`



### 3.3 生成Data文件夹

---

`mysqld --initialize`



### 3.4 启动MySQL服务器

---

`net start 服务器名`

如：`net start mysql`

---

### 3.5 关闭 MySQL 服务器

`net stop 服务器名`

如：`net stop mysql`

---

### 3.6 开启MySQL服务器无密码模式

`mysqld --skip-grant-tables`

（光标会一直闪烁，不要动，打开另一个命令行窗口）

---

### 3.7 登录MySQL服务器

在新的dos窗口中输入

`mysql -u root -p`

密码默认为 root

登录成功

---

### 3.8 更改Mysql服务器密码

在服务器中输入 `use mysql` 进入mysql数据库

在服务器中输入 `select user,host,password from user;` 出现错误说明 password 字段已经更改

+ 若未出现错误，则输入

  `update user setpassword = password("Lrf63838076976") where user = "root";`

+ 若出现错误，则输入

  `update user set authentication_string = password("Lrf63838076976") where user = "root"; `

退出mysql服务器重新启动

在服务器中输入 `alter user user() identified by "Lrf63838076976";`

---

### 3.9 查看Mysql服务器版本

- 在命令行中输入 `mysql --version` 或`mysql -V`
- 在数据库中输入 `select version();`







---

## 4. Mysql常见命令

---

### 4.1 数据库操作

---

#### 4.1.1 数据库操作

- `select database();`
  显示当前数据库名
  <br>
  
- `show databases;`
  展示整个数据库中所有的分数据库
  <br>
  
- `use 数据库名;`
  切换到指定分数据库
  <br>
  
- 创建指定数据库
  
  ```mysql
  create database 数据库名;
  ```
  
  创建一个使用utf8字符集的指定数据库
  `create database 数据库名 character set utf8;`
  
  `create database 数据库名 character set utf8 collate utf8_bin;`
  创建一个使用utf8字符集，并带utf8_bin校对规则的指定数据库
  <br>
  
- 删除指定数据库
  
  ```mysql
  drop database 数据库名;
  ```
  
- 查看创建数据库时的创建语句

  ```mysql
  show create database 数据库名;
  ```

  

#### 4.1.2 数据库备份和恢复



---

### 4.2 表操作

---

#### 4.2.1 表操作

- 展示数据库中所有表

  ```mysql
  show tables;
  ```

- 展示指定数据库中所有表
  
  ```mysql
  show tables from 数据库名;
  ```

+ 创建指定表
  创建表时如果没有指定字符集和校对规则，则表使用数据库的字符集和校对规则

  ```mysql
  create table 表名(
    列名 列数据类型名(可选类型长度) 可选约束条件,
    列名 列数据类型名(可选类型长度) 可选约束条件,
    列名 列数据类型名(可选类型长度) 可选约束条件,
    列名 列数据类型名(可选类型长度) 可选约束条件,
    列名 列数据类型名(可选类型长度) 可选约束条件,
    列名 列数据类型名(可选类型长度) 可选约束条件
  );
  ```

+ 删除指定表

  ```mysql
  drop table 表名;
  ```

+ 查看创建表时的创建语句

  ```mysql
  show create table 表名;
  ```

+ 展示指定表的结构

  ```mysql
  desc 表名;
  ```

+ 展示指定表的数据

  ```mysql
  select * from 表名;
  ```

  

#### 4.2.2 表数据操作

##### 4.2.2.1 增

- 向指定表中插入单条数据

  ```mysql
  insert into 表名
  -> VALUES
  -> (值,值,值);
  ```

  ```mysql
  insert into 表名
  -> (列名,列名,列名)
  -> VALUES
  -> (值,值,值);
  ```

- 向指定表中插入多条数据

  ```mysql
  insert into 表名
  -> VALUES
  -> (值,值,值),(值,值,值),(值,值,值);
  ```

  ```mysql
  insert into 表名
  -> (列名,列名,列名)
  -> VALUES
  -> (值,值,值),(值,值,值),(值,值,值);
  ```

+ 

  ```mysql
  alter table 表名 add column 新列名 新列数据类型名 not null
  ```



##### 4.2.2.2 删

+ 删除指定表中的指定记录
  未指定where子句表中记录将全部删除

  ```mysql
  delete from 表名 [可选where子句];
  ```

+ 删除指定表的数据，保留表的结构

  ```mysql
  truncate table 表名;
  ```

- 删除指定表的数据和结构

  ```mysql
  drop table 表名;
  ```

  

##### 4.2.2.3 改

- 修改指定表中的指定字段

  ```mysql
  update 表名 set 列名=新值语句,列名=新值语句, 列名=新值语句[可选where子句];
  ```





##### 4.2.2.4 查

- 从指定表中查找指定数据

  ```mysql
  select 列名,列名 from 表名[可选where子句[可选like子句]][LIMIT N][OFFSET M];
  ```

- 从指定表中查找指定无重复数据

  ```mysql
  select distinct 列名 from 表名
  ```



一：Where子句的匹配方式

1. 单条件

- `select * from 表名 where 列名 = 字面量`

2. 多条件

- `select * from 表名 where 列名 = 字面量 and 列名 = 字面量`
  和(and)

- `select * from 表名 where 列名 = 字面量 or 列名 = 字面量`
  或(or)

3. 成员匹配

- `select * from 表名 where 列名 in（字面量1,字面量2)`
  in

4. 取两者之间

- `select * from 表名 where 列名 between 1 and 20`
  between ... and

<br>



二：Like子句的匹配方式

- `select * from 表名 where 列名 like 模糊匹配字面量;`

1. 模糊匹配(仅限于like子句)

- %
  表示任意 0 个或多个字符。可匹配任意类型和长度的字符，有些情况下若是中文，请使用两个百分号（%%）表示。
- _
  表示任意单个字符。匹配单个任意字符，它常用来限制表达式的字符长度语句。
- []
  表示括号内所列字符中的一个（类似正则表达式）。指定一个字符、字符串或范围，要求所匹配对象为它们中的任一个。
- [^] 
  表示不在括号所列之内的单个字符。其取值和 [] 相同，但它要求所匹配对象为指定字符以外的任一个字符。
  查询内容包含通配符时,由于通配符的缘故，导致我们查询特殊字符 "%"、"_"、"[" 的语句无法正常实现，而把特殊字符用 "[ ]" 括起便可正常查询

模糊匹配字面量举例：

- `"%f"`     //以f结尾的数据
- `"f%"`     //以f开头的数据
- `"%f%"`    //含有f的数据
- `"_f_"`    //三位且中间字母是f的数据
- `"__f"`    //两位且结尾字母是f的数据
- `"f__"`    //两位且开头字母是f的数据
- `"_f%"`    //第一位任意,第二位为f,后面几位任意的数据
- `"%f_"`    //前面几位任意，倒数第二位为f，倒数第一位任意的数据
- `"__f%"`   //第一第二位任意,第三位为f,后面几位任意的数据
- `"%f__"`   //前面几位任意，倒数第三位为f，倒数第二第一位任意的数据
  <br>



三：UNION和UNION ALL操作符

- `select 列名 from 表名 联合操作符 select 列名 from 表名;`

1. UNION

- `select 列名 from 表名 union select 列名 from 表名;`
  选取所有不同的列数据

2. UNION ALL

- `select 列名 from 表名 union all select 列名 from 表名;`
  选取包括相同的所有列数据

3. 带有where的联合操作符

- `select 列名 from 表名 where 列名 = 字面量 union select 列名 from 表名 where 列名 = 字面量;`
- `select 列名 from 表名 where 列名 = 字面量 union all select 列名 from 表名 where 列名 = 字面量;`

<br>



四：order by子句的排序方式

- `select * from 表名 order by 列名 排序方式`

1. ASC(默认)

- `select * from 表名 order by 列名 ASC`
  升序排序指定列数据

2. DESC(手动)

- `select * from 表名 order by 列名 DESC`
  降序排序指定列数据

3. 多条件排序

- `select * from 表名 order by 列名 排序方式,列名 排序方式,列名 排序方式`

注：
如果字符集采用的是 gbk(汉字编码字符集)，直接在查询语句后添加 order by
select * from 表名 order by 列名
如果字符集采用的是 utf8(万国码)，需要先对字段进行转码然后排序
select * from 表名 order by convert(列名 using gbk)
<br>



五：REGEXP运算符与正则表达式

- `select 列名 from 表名 where 列名 regexp "正则表达式"`

模式：

- `"^st"`
  以"st"开头的所有数据
- `"ok$"`
  以"ok"结尾的所有数据
- `"."`
  除"\n"以外的任何单个字符
- `"[abc]"`                  字符集合
  匹配字符集合中的任意一个字符
  举例："[abc]"可以匹配"plain"中的"a"
- `"[^abc]"`                 负值字符集合
  匹配除字符集合中的任意一个字符
- `"p1|p2|p3"`
  匹配p1或p2或p3
  举例："z|food"能匹配"z"或"food"。"(z|f)ood"能匹配"zood"或"food"
- `"zoo*"`
  匹配前面的子表达式零次或多次
  举例："zoo*"能匹配"z"、"zo"、"zoo"
- `"zoo+"`
  匹配前面的子表达式一次或多次
  举例："zoo+" 能匹配"zo"、"zoo"，但不能匹配"z"
- `"o{n}"`
  n是一个非负整数，匹配确定的n次
  举例："o{2}" 不能匹配 "Bob" 中的 "o"，但是能匹配 "food" 中的两个o
- `"o{n,m}"`
  n和m是两个非负整数，其中n<=m。最少匹配n次且最多匹配m次
  举例："o{2,5}" 不能匹配 "Bob" 中的 "o"，不能匹配 "Yoooooo" 中的六个o，但是能匹配 "food" 中的两个o
  <br>



六：分组查询

多个子句的顺序

```sql
select * from 表名 
        group by 列名
        having 条件表达式
        order by 列名
        limit 开始, 结束
```

```sql
select 列名1, 列名2, 列名3 ... from 表名 
        group by 列名
        having 条件表达式
        order by 列名
        limit 开始, 结束
```

```sql
select 函数名1(), 函数名2(), 函数名3() ... from 表名 
        group by 列名
        having 条件表达式
        order by 列名
        limit 开始, 结束
```

<br>

七：分页查询

- `select * from 表名 limit n1 , n2`
  n1：每页显示记录数 * (第几页-1)
  n2：每页显示记录数

---

#### 

八：

- `select * f`

---



#### 4.2.3 用户表操作

+ 查看位于mysql数据库中的user用户表，用于进行权限操作

  ```mysql
  select * from mysql.user;
  ```



---



### 4.3 字符集操作

- 列出mysql所支持的所有字符集
  
  ``` mysql
  show character set;
  ```
  
- 当前mysql服务器的字符集设置
  
  ```mysql
  show variables like "character_set_%";
  ```
  
- 当前mysql服务器字符集校验设置
  
  ```mysql
  show variables like "collation_%;
  ```
  
- 显示某数据库字符集设置
  
  ```mysql
  show create database 数据库名;
  ```
  
- 显示某数据表字符集设置
  
  ```mysql
  show create table 表名;
  ```
  
- 修改数据库的字符集以支持中文
  
  ```mysql
  alter database 数据库名 default character set 'utf8';
  ```
  
- 修改表的字符集以支持中文
  
  ```mysql
  alter table 表名 convert to character set utf8;
  ```







## 5. Mysql运算符

---

### 5.1 算术运算符

- `+`                         加法
- `-`                         减法
- `*`                         乘法
- `/或DIV`                    除法
- `%或MOD`                    取余

注：DIV和MOD为函数

---

### 5.2 比较运算符

- `=`                        等于
  注：比较两个null值时结果为NULL,比较两个非null值时结果为0
- `!=`                       不等于
- `>`                        大于
- `<`                        小于
- `>=`                       大于等于
- `<=`                       小于等于
- `<=>`                      严格比较两个NULL值是否相等
  注：两个操作码均为NULL时，其所得值为1；而当一个操作码为NULL时，其所得值为0
  <br>

- `BETWEEN`                  在两值之间
  `select 5 between 1 and 10`
- `NOT BETWEEN`              不在两值之间
  `select 5 not between 1 and 10`
- `IN`                       在集合中
  `select 5 in (1,2,3,4,5)`
- `NOT IN`                   不在集合中
  `select 5 not in (1,2,3,4,5`
  <br>

- `LIKE`                     模糊匹配
  `select '12345' like '12%'`
- `REGEXP 或 RLIKE`            正则式匹配
  `select 'beijing' REGEXP 'bei'`
- `IS NULL`                  为空
- `IS NOT NULL`              不为空
- ``
  注：null值为空得1，非null值不为空得0

---

### 5.3 逻辑运算符

- `!或NOT`                   逻辑非
- `AND`                      逻辑与
- `OR`                       逻辑或
- `XOR`                      逻辑异或

---

### 5.4 位运算符

- `&`                        按位与
- `|`                        按位或
- `^`                        按位异或
- `~`                        按位取反
- `>>`                       按位右移
- `<<`                       按位左移







---

## 6. Mysql语句

---

### 6.1 选择语句

```mysql
select case <单值表达式>
when 表达式1 then 表达式2(SQL语句或返回值)
when 表达式3 then 表达式4(SQL语句或返回值)
when 表达式5 then 表达式6(SQL语句或返回值)
else 表达式7(SQL语句或返回值)
end

# 如果表达式1为true，则返回表达式2
# 如果表达式3为true，则返回表达式4
# 如果表达式5为true，则返回表达式6
# 否则返回表达式7
```







---

## 7. Mysql函数

---

### 7.1 字符串函数

+ 返回指定字符串的字符集

  ```mysql
  select charset(str);
  select charset(str) from 表名;

- 
  按字节返回指定字符串的长度
  
  ```mysql
  select length(string1);
  select length(string1) from 表名;
  ```
  
- 
  连接指定的N个字符串
  
  ```mysql
  select concat(string1,string2,string3...stringN);
  select concat(string1,string2,string3...stringN) from 表名;
  ```
  
- 
  按整数返回string2在string1中第一次出现的位置，没有返回0

  ```mysql
  select instr(string1,string2);
  select instr(string1,string2) from 表名;
  ```

- 逐字符比较两字符串大小
  
  ```mysql
  select strcmp(string1,string2);
  select strcmp(string1,string2) from 表名;
  ```

- 从string1的位置position(默认为1)开始截取数量length的字符

  ```mysql
  select substring(string1,position);                                          # 当length忽略时一直取到结尾
  select substring(string1,position,length) from 表名
  ```

  


- 从string1的左边起取数量length的字符

  ```mysql
  select left(string1,length);
  select left(string1,length) from 表名;
  ```

- 从string1的右边起取length个字符

  ```mysql
  select right(string1,length);
  select right(string1,length) from 表名;
  ```



+ 将指定字符串转换为大写

  ```mysql
  select ucase(string1)
  select ucase(string1) from 表名
  ```

+ 将指定字符串转换为小写

  ```mysql
  select lcase(string1)
  select lcase(string1) from 表名
  ```




- 寻找string1中所有能匹配search_str的字符串，并用replace_str替换他们

  ```mysql
  select replace(string1,search_str,replace_str)
  select replace(string1,search_str,replace_str) from 表名
  ```

  

+ 去除指定字符串前端的空格

  ```mysql
  select ltrim(string1)
  select ltrim(string1) from 表名
  ```

- 去除指定字符串后端的空格

  ```mysql
  select rtrim(string1)
  select rtrim(string1) from 表名
  ```

- 去除指定字符串前后两端的空格

  ```mysql
  select trim(string1)
  select trim(string1) from 表名
  ```

  

---

### 7.2 数学函数

- `select abs(number1)`
  `select abs(number1) from 表名`
  取数值的绝对值
  <br>

- `select ceiling(number1)`
  `select ceiling(number1) from 表名`
  向上取整，得到比number1大的最小整数
  <br>

- `select floor(number1)`
  `select floor(number1) from 表名`
  向下取整，得到比number1小的最大整数
  <br>

- `select format(number1,number2)`
  `select format(number1,number2) from 表名`
  保留小数位数(四舍五入)，对number1四舍五入保留number2位的小数位数(以科学计数法)
  <br>



- `least(number1,number2,...,numberN)`
  `least(number1,number2,...,numberN) from 表名`
  求最小值，返回N个数值中的最小值
  <br>

- `select conv(number1,from_base,to_base)`
  `select conv(number1,from_base,to_base) from 表名`
  进制转换，把from_base进制的number1转换为to_base进制
  <br>

- `select bin(number1)`
  `select bin(number1) from 表名`
  十进制数值转为二进制数值
  <br>

- `select hex(number1)`
  `select hex(number1) from 表名`
  十进制数值转为十六进制数值
  <br>

- `select rand(seed)`
  `select rand(seed) from 表名`
  返回随机数，其范围为 0 <= v <= 1.0
  seed为数值
  如果省略seed，生成随机的随机数
  如果不省略seed，生成固定的随机数
  <br>

---

### 7.3 日期函数

- `select current_date()`
  返回当前日期
  <br>

- `select current_time()`
  返回当前时间
  <br>

- `select current_timestamp()`
  `select now()`
  返回当前时间戳(语句开始执行时的时间)
  `select sysdate()`
  返回当前时间戳(函数开始执行时的时间)
  <br>



- `select date(datetime)`
  返回datetime的日期部分
  <br>

- `select date_add(date1,interval 时间值 时间类型 )`
  在date2中加上日期或时间
  `select date_sub(date1,interval 时间值 时间类型)`
  在date2中减去日期或时间
  时间类型可以是year、minute、hour、day等
  <br>举例：
  where date_add(send_time,interval 10 minute) >= now()
  对send_time中所有时间加上10分钟(在发送时间中寻找离现在不超过10分钟的行)
  where send_time >= date_sub(now(),interval 10 minute)
  对现在的时间减去10分钟(在发送时间中寻找离现在不超过10分钟的行)
  <br>

- `select datediff(date1,date2)`
  返回两个日期差(结果类型是date)
  <br>

- `select timediff(time1,time2)`
  返回两个时间差(结果类型是time)
  <br>



- `select year(datetime);`
  返回datetime的年部分
  <br>

- `select month(datetime);`
  返回datetime的月部分
  <br>

- `select day(datetime);`
  返回datetime的日部分
  <br>

- `select unix_timestamp();`
  返回1970-1-1到现在的秒数
  `select from_unixtime(unix_timestamp,"%Y-%m-%d")`
  `select from_unixtime(unix_timestamp,"%Y-%m-%d %H:%i:%s")`
  把一个整形秒数转换为指定格式的日期
  <br>

---

### 7.4 高级函数

---

#### 7.4.1 加密函数

- `select md5(str);`
  常用于应用和web程序，加密后32位，加密不可逆
  <br>

- `select password();`
  常用于mysql的用户密码，加密后旧版16位，新版41位，加密不可逆
  mysql管理系统的用户密码就是默认用password()加密的
  <br>

#### 7.4.2 流程控制函数

- `select if(表达式1,表达式2,表达式3)`
  如果表达式1为true，返回表达式2，否则返回表达式3
  <br>

- `select ifnull(表达式1,表达式2)`
  如果表达式1不为空NULL，则返回表达式1，否则返回表达式2
  注：表达式中判断是否为空要用is null不要用等号
  <br>

#### 7.4.3 系统函数

- `select user()`
  查询用户
  查询当前登录到mysql的所有用户以及登录IP
  <br>

- `select database()`
  显示当前使用的数据库名称
  <br>

---

#### 7.4.4 统计函数

注：指定列数据类型必须为数值类型，否则无意义

- `select count(*) from 表名`
  统计表中总记录数
  `select count(列名) from 表名`
  统计表中指定列非空的总记录数
  <br>
- `select sum(列名) from 表名`
  统计指定列的所有数据之和
  `select sum(列名+列名+列名) from 表名`
  统计指定几列的所有数据之和
  <br>
- `select avg(列名) from 表名`
  统计指定列的所有数据的平均数
  `select avg(列名+列名+列名) from 表名`
  统计指定几列的所有数据的平均数
- `select max(列名) from 表名`
  统计指定列的所有数据的最大值
  `select max(列名+列名+列名) from 表名`
  统计指定几列的所有数据的最大值
- `select min(列名) from 表名`
  统计指定列的所有数据的最小值
  `select min(列名+列名+列名) from 表名`
  统计指定几列的所有数据的最小值
  <br>



```markdown
(1) select 
```

