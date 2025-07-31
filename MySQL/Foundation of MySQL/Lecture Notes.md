<h1 sytle="text-align:ceneter;">MySQL 基础</h1>

下面内容都是我的学习笔记。

# 第一章 初识 MySQL

## 第一节 数据库基础知识

### 1、数据库

#### 1.1 概念

数据库，英文名称为 Database，简称 DB。从字面意思来看就是存储数据的仓库；从专业角度解释为存储在计算机磁盘上的有组织、可共享的大量数据的集合。

![img1](./img/img1.png#pic_center)

#### 1.2 数据库的类型

数据库分为 **关系型数据库** 和 **非关系型数据库** 两大类。常见的关系型数据库有 `MySQL`、`Oracle`、`SQL Server`、`SQLite`、`DB2` 等。常见的非关系型数据库有 `Redis`、`MongoDB` 等。

#### 1.3 数据库管理系统

数据库管理系统，英文名称为 Database Management System，简称 DBMS。主要用于科学组织和存储数据、高效的获取和维护数据。比如 `Navicat`。

### 2、MySQL 介绍和安装

#### 2.1 MySQL 介绍

- `MySQL` 是目前最流行的开源的、免费的关系型数据库，适用于中小型甚至大型互联网应用，能够在 Windows 和 Linux 平台上部署。
- MySQL Oracle 属于Oracle公司。

#### 2.2 MySQL 安装

略

#### 2.3 连接数据库

找到 MySQL 安装目录下的 bin 目录，然后打开命令窗口，在命令窗口中按如下语法输入命令：

```
mysql -h MySQL数据库服务器的IP地址 -u 用户名 -p
```

然后按下回车键，输入密码即可。

## 第二节 结构化查询语言

### 1、SQL 分类

结构化查询语句，英文名称为 Structured Query Language，简称 `SQL`。结构化查询语句分为数据定义语言、数据操作语言、数据查询语言和数据控制语言四大类。

| 名称 | 描述 | 命令 |
| ---- | --- | --- |
| 数据定义语言 (`DDL`) | 数据库、数据表的创建、修改和删除 | `CREATE`、`ALTER`、`DROP` |
| 数据操作语言 (`DML`) | 数据的增加、修改和删除 | `INSERT`、`UPDATE`、`DELETE` |
| 数据查询语言 (`DQL`) | 数据的查询 | `SELECT` |
| 数据控制语言 (`DCL`) | 用户授权、事务的提交和回滚 | `GRANT`、`COMMIT`、`ROLLBACK` |

### 2、数据库操作

#### 2.1 创建数据库的语法

```sql
CREATE DATABASE [IF NOT EXISTS] 数据库名称 DEFAULT CHARACTER SET 字符集 COLLATE 排序规则;
```

示例：创建数据库 `lesson`，并指定字符集为 `GBK`，排序规则为 `GBK_CHINESE_CI`

```sql
CREATE DATABASE IF NOT EXISTS lesson DEFAULT CHARACTER SET GBK COLLATE GBK_CHINESE_CI;
```

#### 2.2 修改数据库的语法

```sql
ALTER DATABASE 数据库名称 CHARACTER SET 字符集 COLLATE 排序规则;
```

示例：修改数据库 `lesson` 的字符集为 `UTF8`，排序规则为 `UTF8_GENERAL_CI`

```sql
ALTER DATABASE lesson CHARACTER SET UTF8 COLLATE UTF8_GENERAL_CI;
```

#### 2.3 删除数据库的语法

```sql
DROP DATABASE [IF EXISTS] 数据库名称;
```

示例：删除数据库 `lesson`

```sql
DROP DATABASE IF EXISTS lesson;
```

#### 2.4 查看数据库语法

```sql
SHOW DATABASES;
```

#### 2.5 使用数据库的语法

```sql
USE 数据库名称;
```

示例：使用数据库 `lesson`

```sql
USE lesson;
```

### 3、列类型

#### 3.1 数值类型

| 类型 | 说明 | 取值范围 | 存储需求 |
| --- | --- | --- | --- |
| `tinyint` | 非常小的数据 | 有符号值: $-2^7$ ~ $2^7-1$ 无符号值: $0$ ~ $2^8-1$ | $1$ 字节 |
| `smallint` | 较小的数据 | 有符号值: $-2^{15}$ ~ $2^{15}-1$ 无符号值: $0$ ~ $2^{16}-1$ | $2$ 字节 |
| `mediumint` | 中等大小的数据 | 有符号值: $-2^{23}$ ~ $2^{23}-1$ 无符号值: $0$ ~ $2^24-1$ | $3$ 字节 |
| `int` | 标准整数 | 有符号值: $-2^{31}$ ~ $2^{31}-1$ 无符号值: $0$ ~ $2^{32}-1$ | $4$ 字节 |
| `bigint` | 较大的整数 | 有符号值: $-2^{63} ~ 2^{63}-1$ 无符号值: $0$ ~ $2^{64}-1$ | $8$ 字节 |
| `float` | 单精度浮点数 | 无符号值: $1.1754351 \times 10^{-38}$ ~ $3.402823466 \times 1038$ | $4$ 字节 |
| `double` | 双精度浮点数 | 无符号值: $2.22507385 \times 10^{-308}$ ~ $1.79769313\times 10^{308}$ | $8$ 字节 |
| `decimal` | 字符串形式的浮点数 | `decimal(m, d)` | $m$ 个字节 |

#### 3.2 日期时间类型

| 类型 | 说明 | 取值范围 |
| --- | --- | --- |
| `DATE` | `YYYY-MM-DD`，日期格式 | `1000-01-01` ~ `9999-12-31` |
| `TIME` | `HH:mm:ss`，时间格 | `-838:59:59.000000` ~ `838:59:59.000000` |
| `DATETIME` | `YY-MM-dd HH:mm:ss` | `1000-01-01 00:00:00.000000` ~ `9999-12-31 23:59:59.999999` |
| `TIMESTAMP` | `YYYY-MM-dd HH:mm:ss` 格式表示的时间戳 | `1970-01-01 00:00:01.000000` ~ `2038-01-19 03:14:07.999999` |
| `YEAR` | `YYYY` 格式的年份值 | `1901` ~ `2155` |

#### 3.3 字符串类型

| 类型 | 说明 | 最大长度 |
| --- | --- | --- |
| `char [(M)]` | 固定长字符串，检索快但费空间，$0 <= M <= 255$ | $M$ 字符 |
| `varchar [(M)]` | 可变字符串 $0 <= M <= 65535$ | 变长度 |
| `text` | 文本串 | $2^{16}–1$ 字节 |

#### 3.4 列类型修饰属性

| 属性名 | 说明 | 示例 |
| --- | --- | --- |
| `UNSIGNED` | 无符号，只能修来修饰数值类型，表名该列数据不能出现负数 | `INT(4) UNSIGNED`，表示只能为 $4$ 位大于等于 $0$ 的整数 |
| `ZEROFILL` | 不足的位数使用 $0$ 来填充 | `INT(4) ZEROFILL`，如果给定的值为 $10$，此时只有 $2$ 位，而该列需要 $4$ 位，不足的 $2$ 位由 $0$ 来填充，最终值为 $0010$ |
| `NOT NULL` | 表示该列类型的值不能为空 | `VARCHAR (20) NOT NULL`，表示该列数据不能为空值 |
| `DEFAULT` | 表示设置默认值 | `INT(4) DEFAULT 0`，表示该列不赋值时默认为 $0$ |
| `AUTO_INCREMENT` | 表示自增长，只能应用于数值列类型，该列类型必须为键，且不能为空 | `INT(11) AUTO_INCREMENT NOT NULL PRIMARY KEY`。第一次为该列中插入值时为 $1$，第二次为 $2$ |

### 4、数据表操作

#### 4.1 数据表类型

`MySQL` 中的数据表类型有很多种，如 `MyISAM`、`InnoDB`、`HEAP`、`BOB`、`CSV` 等。其中最常用的就是 `MyISAM` 和 `InnoDB`。

#### 4.2 MyISAM 与 InnoDB 的区别

| 名称 | `MyISAM` | `InnoDB` |
| :---: | :---: | :---: |
| 事务处理 | 不支持 | 支持 |
| 数据行锁定 | 不支持 | 支持 |
| 外键约束 | 不支持 | 支持 |
| 全文索引 | 支持 | 不支持 |
| 表空间大小 | 较小 | 较大，约2倍 |

- 事务：涉及的所有操作是一个整体，要么都执行，要么都不执行（原子性）。
- 数据行锁定：一行数据，当一个用户在修改该数据时，可以直接将该条数据锁定。
- 如何选择数据表的类型：当涉及的业务操作以查询居多，修改和删除较少时，可以使用 `MyISAM`。当涉及的业务操作经常会有修改和删除 操作时，使用 `InnoDB`。

#### 4.3 创建数据表

```sql
CREATE TABLE [IF NOT EXISTS] 数据表名称(
    字段名1 列类型(长度) [修饰属性] [键/索引] [注释],
    字段名2 列类型(长度) [修饰属性] [键/索引] [注释],
    字段名3 列类型(长度) [修饰属性] [键/索引] [注释],
    ......
    字段名n 列类型(长度) [修饰属性] [键/索引] [注释]
) [ENGINE = 数据表类型][CHARSET=字符集编码] [COMMENT=注释];
```

示例：创建学生表，表中有字段学号、姓名、性别、年龄和成绩。

```sql
CREATE TABLE IF NOT EXISTS student(
    `number` VARCHAR(30) NOT NULL PRIMARY KEY COMMENT '学号，主键',
    `name` VARCHAR(30) NOT NULL COMMENT '姓名',
    `sex` TINYINT(1) UNSIGNED DEFAULT 0 COMMENT '性别：0-男 1-女 2-其他',
    `age` TINYINT(3) UNSIGNED DEFAULT 0 COMMENT '年龄',
    `score` DOUBLE(5, 2) UNSIGNED COMMENT '成绩'
)ENGINE=InnoDB CHARSET=UTF8 COMMENT='学生表';
```

#### 4.4 修改数据表

- **修改表名**
	```sql
	ALTER TABLE 表名 RENAME AS 新表名;
	```
	示例：将 `student` 表名称修改为 `stu`。
	```sql
	ALTER TABLE student RENAME AS stu;
	```
- **增加字段**
	```sql
	ALTER TABLE 表名 ADD 字段名 列类型(长度) [修饰属性] [键/索引] [注释];
	```
	示例：在 `stu` 表中添加字段联系电话 (`phone`)，类型为字符串，长度为 $11$，非空。
	```sql
	ALTER TABLE stu ADD phone VARCHAR(11) NOT NULL COMMENT '联系电话';
	```
- 查看表结构
	```sql
	DESC 表名; -- 查看结构
	```
- **修改字段**
	```sql
	-- MODIFY 只能修改字段的修饰属性
	ALTER TABLE 表名 MODIFY 字段名 列类型(长度) [修饰属性] [键/索引] [注释];

	-- CHANGE 可以修改字段的名字以及修饰属性
	ALTER TABLE 表名 CHANGE 字段名 新字段名 列类型(长度) [修饰属性] [键/索引] [注释];
	```
	示例： 将 `stu` 表中的 `sex` 字段的类型设置为 `VARCHAR`，长度为 $2$，默认值为 `男`，注释为 `性别，男，女，其他`。
	```sql
	ALTER TABLE stu MODIFY sex VARCHAR(2) DEFAULT '男' COMMENT '性别：男，女，其他';
	```
	示例：将 `stu` 表中 `phone` 字段修改为 `mobile`，属性保持不变。
	```sql
	ALTER TABLE stu CHANGE phone mobile VARCHAR(11) NOT NULL COMMENT '联系电话';
	```
- **删除字段**
	```sql
	ALTER TABLE 表名 DROP 字段名;
	```
	示例：将 `stu` 表中的 `mobile` 字段删除。
	```sql
	ALTER TABLE stu DROP mobile;
	```

#### 4.5 删除数据表

```sql
DROP TABLE [IF EXISTS] 表名;
```

示例：删除数据表 `stu`。

```sql
DROP TABLE IF EXISTS stu;
```

# 第二章 MySQL 数据的增删改查

## 课前回顾

**1. 在数据库 exercise 中创建课程表 stu_course，包含字段课程编号 (number)，类型为整数，长度为 11，是主键，自增长，非空；课程名称 (name)，类型为字符串，长度为20，非空；学分 (score)，类型为浮点数，小数点后面保留2位有效数字，长度为5，非空。**

```sql
-- 如果数据库不存在就创建数据库
CREATE DATABASE IF NOT EXISTS exercise DEFAULT CHARACTER SET UTF8 COLLATE UTF8_GENERAL_CI;

-- 使用数据库
USE exercise;

-- 在数据库钟创建数据表 stu_course
CREATE TABLE IF NOT EXISTS stu_course (
	`number` INT(11) AUTO_INCREMENT PRIMARY KEY NOT NULL COMMENT '课程编号',
	`name` VARCHAR(20) NOT NULL COMMENT '课程名称',
	`score` DOUBLE(5, 2) NOT NULL COMMENT '学分'
) ENGINE=InnoDB CHARSET=UTF8 COMMENT '课程表';
```

**2. 将课程表重命名为 course。**

```sql
ALTER TABLE stu_course RENAME AS course;
```

**3. 在课程表中添加字段学时 (time)，类型为整数，长度为 3，非空。**

```sql
ALTER TABLE course
ADD `time` INT(3) NOT NULL COMMENT '学时'; 
```

**4. 修改课程表学分类型为浮点数，小数点后面保留 1 位有效数字，长度为 3，非空。**

```sql
ALTER TABLE course
MODIFY `score` DOUBLE (3, 1) NOT NULL COMMENT '学分';
```

**5. 删除课程表**

```sql
DROP DATABASE IF EXISTS course;
```

**6. 删除数据库 exercise**

```sql
DROP DATABASE IF EXISTS exercise;
```

## 第一节 DML 语句

### 1、什么是 DML
DML (Data Manipulation Language)：数据操作语言。主要体现于对表数据的增删改操作。因此 DML 仅包括 `INSERT`、`UPDATE` 和 `DELEETE` 语句。

### 2、INSERT 语句

```sql
-- 需要注意，VALUES 后的字段值必须与表名后的字段名一一对应
INSERT INTO 表名(字段名1, 字段名2, ..., 字段名n) VALUES(字段值1, 字段值2, ..., 字段值 n);

-- 需要注意，VALUES 后的字段值必须与创建表时的字段顺序保持一一对应
INSERT INTO 表名 VALUES(字段值1, 字段值2, ..., 字段值n);

-- 一次性插入多条数据
INSERT INTO 表名(字段名1, 字段名2, ..., 字段名n) VALUES(字段值1, 字段值2, ..., 字段值 n),(字段值1, 字段值2, ..., 字段值n), ... , (字段值1, 字段值2, ..., 字段值n); INSERT INTO 表名 VALUES(字段值1, 字段值2, ..., 字段值n), (字段值1, 字段值2, ..., 字段值 n), ..., (字段值1, 字段值2, ..., 字段值n);
```

**示例**：向课程表中插入数据。

```sql
INSERT INTO course(`number`, name, score, `time`) VALUES (1, 'Java基础', 4, 40);
INSERT INTO course VALUES (2, '数据库', 3, 20);
INSERT INTO course(`number`, score, name, `time`) VALUES (3, 5, 'Jsp', 40);
INSERT INTO course(`number`, name, score, `time`) VALUES (4, 'Spring', 4, 5), (5, 'Spring Mvc', 2, 5);
INSERT INTO course VALUES (6, 'SSM', 2, 3), (7, 'Spring Boot', 2, 2);
```

### 3、UPDATE 语句

```sql
UPDATE 表名 SET 字段名1=字段值1,[字段名2=字段值2, ..., 字段名n=字段值n] [WHERE 修改条件]
```

#### 3.1 WHERE条件子句

- 在 Java 中，条件的表示通常都是使用关系运算符来表示，在 SQL 语句中也是一样，使用 `>`, `<`, `>=`, `<=`, `!=` 来表示。不同的是，除此之外，SQL 中还可以使用 SQL 专用的关键字来表示条件。这些将在后面的 DQL 语句中详细讲解。
- 在 Java 中，条件之间的衔接通常都是使用逻辑运算符来表示，在 SQL 语句中也是一样，但通常使用 AND 来表示逻辑与 (`&&`)，使用 OR 来表示逻辑或 (`||`)。

**示例**

```sql
WHERE time > 20 && time < 40; <=> WHERE time > 20 and time < 40;
```

#### 3.2 UPDATE 语句

**示例**：将数据库的学分更改为 $4$，学时更改为 $15$。

```sql
UPDATE course SET score=4, `time`=15 WHERE name='数据库';
```

### 4、DELETE语句

```sql
DELETE FROM 表名 [WHERE 删除条件];
```

**示例**：删除课程表中课程编号为 1 的数据。

```sql
DELETE FROM course WHERE `number`=1;
```

### 5、TRUNCATE语句

```sql
-- 清空表中数据
TRUNCATE [TABLE] 表名;
```

**示例**：清空课程表数据。

```sql
TRUNCATE course;
```

### 6、DELETE 与 TRUNCATE 区别

- DELETE 语句根据条件删除表中数据，而 TRUNCATE 语句则是将表中数据全部清空；如果 DELETE 语句要删除表中所有数据，那么在效率上要低于 TRUNCATE 语句。
- 如果表中有自增长列，TRUNCATE 语句会重置自增长的计数器，但 DELETE 语句不会。
- TRUNCATE 语句执行后，数据无法恢复，而 DELETE 语句执行后，可以使用事务回滚进行恢复。

## 第二节 DQL 语句

### 1、什么是 DQL

DQL (Data Query Language)：数据查询语言。体现在数据的查询操作上，因此，DQL 仅包括 SELECT 语句。

### 2、SELECT语句

```sql
SELECT ALL/DISTINCT * | 字段名1 AS 别名1[,字段名2 AS 别名2, ..., 字段名n AS 别名n] FROM 表名 WHERE 查询条件
```

**解释说明**：ALL 表示查询所有满足条件的记录，可以省略；DISTINCT 表示去掉查询结果中重复的记录；AS 可以给数据列、数据表取一个别名。

**示例**：从课程表中查询课程编号小于5的课程名称。

```sql
SELECT `name` FROM course WHERE `number` < 5;
```

**示例**：从课程表中查询课程名称为"Java基础"的学分和学时。

### 3、比较操作符

| 操作符 | 语法 | 说明 |
| :---: | --- | --- |
| IS NULL | 字段名 IS NULL | 如果字段的值为 NULL，则条件满足。 |
| IS NOT NULL | 字段名 IS NOT NULL | 如果字段的值不为 NULL，则条件满足。|
| BETWEEN...AND | 字段名 BETWEEN 最小值 AND 最大值 | 如果字段的值在最小值与最大值之间（能够取到最小值和最大值），则条件满足。 |
| LIKE | 字段名 LIKE '%匹配内容%' | 如果字段值包含有匹配内容，则条件满足。|
| IN | 字段名 IN(值1，值2，...， 值n) |如果字段值在值1,值2, ...，值n中，则条件满足。 |

**示例**：从课程表查询课程名为 NULL 的课程信息。

```sql
SELECT * FROM course WHERE name IS NULL;
```

**示例**：从课程表查询课程名不为 NULL 的课程信息。

```sql
SELECT * FROM course WHERE name IS NOT NULL;
```

**示例**：从课程表查询学分在 2 ~ 4 之间的课程信息。

```sql
SELECT * FROM course WHERE score BETWEEN 2 AND 4;
```

**示例**：从课程表查询课程名包含"V"的课程信息。

```sql
SELECT * FROM course WHERE name LIKE '%v%';
```

**示例**：从课程表查询课程名以"J"开头的课程信息。

```sql
SELECT * FROM course WHERE name LIKE 'J%'; 
```

**示例**：从课程表查询课程名以"p"结尾的课程信息。

```sql
SELECT * FROM course WHERE name LIKE '%p'; 
```

**示例**：从课程表查询课程编号为 1，3，5 的课程信息。

```sql
SELECT * FROM course WHERE `number` IN (1, 3, 5);
```

### 4、分组

数据表准备：新建学生表 `student`，包含字段学号 (`no`)，类型为长整数，长度为 20，是主键，自增长，非空；姓名 (`name`)，类型为字符串，长度为 20，非空；性别 (`sex`)，类型为字符串，长度为 2，默认值为"男"；年龄 (`age`)，类型为整数，长度为 3，默认值为 0；成绩 (`score`)，类型为浮点数，长度为 5，小数点后面保留 2 位有效数字。

```sql
DROP TABLE IF EXISTS student;
CREATE TABLE student (
	`no` BIGINT(20) AUTO_INCREMENT NOT NULL PRIMARY KEY COMMENT '学号，主键',
	`name` VARCHAR(20) NOT NULL COMMENT '姓名',
	`sex` VARCHAR(2) DEFAULT '男' COMMENT '性别',
	age INT(3) DEFAULT 0 COMMENT '成绩'
) ENGINE=InnoDB CHARSET=UTF8 COMMENT '学生表';
```

插入测试数据：

```sql
INSERT INTO student(no, name, sex, age, score) VALUES (DEFAULT, '张三', '男', 20, 59);
INSERT INTO student(no, name, sex, age, score) VALUES (DEFAULT, '李四', '女', 19, 62);
INSERT INTO student(no, name, sex, age, score) VALUES (DEFAULT, '王五', '其他', 21, 62);
INSERT INTO student(no, name, sex, age, score) VALUES (DEFAULT, '龙华', '男', 22, 75);
INSERT INTO student(no, name, sex, age, score) VALUES (DEFAULT, '金凤', '女', 18, 80);
INSERT INTO student(no, name, sex, age, score) VALUES (DEFAULT, '张华', '其他', 27, 88); 
INSERT INTO student(no, name, sex, age, score) VALUES (DEFAULT, '李刚', '男', 30, 88);
INSERT INTO student(no, name, sex, age, score) VALUES (DEFAULT, '潘玉明', '女', 28, 81);
INSERT INTO student(no, name, sex, age, score) VALUES (DEFAULT, '凤飞飞', '其他', 32, 90);
```

#### 4.1 分组查询

```sql
SELECT ALL/DISTINCT * | 字段名1 AS 别名1[,字段名1 AS 别名1, ..., 字段名n AS 别名n] FROM 表名 WHERE 查询条件 GROUP BY 字段名1，字段名2,..., 字段名n
```

分组查询所得的结果只是该组中的第一条数据。

**示例**：从学生表查询成绩在 $80$ 分以上的学生信息并按性别分组。

```sql
SELECT * FROM student WHERE score > 80 GROUP BY sex;
```

**示例**：从学生表查询成绩在 $60$ ~ $80$ 之间的学生信息并按性别和年龄分组。

```sql
SELECT * FROM student WHERE score BETWEEN 60 AND 80 GROUP BY sex, age;
```

#### 4.2 聚合函数

- `COUNT()`：统计满足条件的数据总条数
	**示例**：从学生表查询成绩在 $80$ 分以上的学生人数。
	```sql
	SELECT COUNT(*) total FROM student WHERE score > 80;
	```
- `SUM()`：只能用于数值类型的字段或者表达式，计算该满足条件的字段值的总和。
	**示例**：从学生表查询不及格的学生人数和总成绩。
	```sql
	SELECT COUNT(*) totalCount, SUM(score) totalScore FROM student WHERE score < 60;
	```
- `AVG()`：只能用于数值类型的字段或者表达式，计算该满足条件的字段值的平均值。
	**示例**：从学生表查询男生、女生、其他类型的学生的平均成绩。
	```sql
	SELECT sex, AVG(score) avgScore FROM student GROUP BY sex;
	```
- `MAX()`：只能用于数值类型的字段或者表达式，计算该满足条件的字段值的最大值。
	**示例**：从学生表查询学生的最大年龄。
	```sql
	SELECT MAX(age) FROM student;
	```
- `MIN()`：只能用于数值类型的字段或者表达式，计算该满足条件的字段值的最小值。
	**示例**：从学生表查询学生的最低分。
	```sql
	SELECT MIN(score) FROM student;
	```

#### 4.3 分组查询结果筛选

```sql
SELECT ALL/DISTINCT * | 字段名1 AS 别名1[,字段名1 AS 别名1, ..., 字段名n AS 别名n] FROM 表名 WHERE 查询条件 GROUP BY 字段名1，字段名2,..., 字段名n HAVING 筛选条件
```

分组后如果还需要满足其他条件，则需要使用 `HAVING` 子句来完成。

**示例**：从学生表查询年龄在 $20$ ~ $30$ 之间的学生信息并按性别分组，找出组内平均分在 $74$ 分以上的组。

```sql
SELECT * FROM student WHERE age BETWEEN 20 AND 30 GROUP BY sex HAVING avg(score) > 74;
```

### 5、排序

```sql
SELECT ALL/DISTINCT * | 字段名1 AS 别名1[,字段名1 AS 别名1, ..., 字段名n AS 别名n] FROM 表名 WHERE 查询条件 ORDER BY 字段名1 ASC|DESC，字段名2 ASC|DESC,..., 字段名n ASC|DESC
```

`ORDER BY` 必须位于 `WHERE` 条件之后。

**示例**：从学生表查询年龄在 $18$ ~ $30$ 岁之间的学生信息并按成绩从高到低排列，如果成绩相同，则按年龄从小到大排列。

```sql
SELECT * FROM student WHERE age BETWEEN 18 AND 30 ORDER BY score DESC, age ASC;
```

### 6、分页

```sql
SELECT ALL/DISTINCT * | 字段名1 AS 别名1[,字段名1 AS 别名1, ..., 字段名n AS 别名n] FROM 表名 WHERE 查询条件 LIMIT 偏移量, 查询条数
```

- `LIMIT` 的第一个参数表示偏移量，也就是跳过的行数。
- `LIMIT` 的第二个参数表示查询返回的最大行数，可能没有给定的数量那么多行。

示例：从学生表分页查询成绩及格的学生信息，每页显示 $3$ 条，查询第 $2$ 页学生信息。

```sql
SELECT * FROM student WHERE score >= 60 LIMIT 3, 3;
```

**注意**：如果一个查询中包含分组、排序和分页，那么它们之间必须按照分组 $\to$ 排序 $\to$ 分页的先后顺序排列。

# 第三章 MySQL 常用函数
