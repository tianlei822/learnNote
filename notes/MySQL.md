

# 一、数据库基础

## 1. 数据库的好处

1. 持久化数据到本地
2. 可以实现结构化查询，方便管理

## 2. 数据库相关概念

1. **DB：** 数据库，保存一组有组织的数据的容器
2. **DBMS：** 数据库管理系统，又称为数据库软件（产品），用于管理DB中的数据
3. **SQL:** 结构化查询语言，用于和DBMS通信的语言

## 3. 数据库存储数据的特点

1. 将数据放到表中，表再放到库中
2. 一个数据库中可以有多个表，每个表都有一个的名字，用来标识自己。表名具有唯一性
3. 表具有一些特性，这些特性定义了数据在表中如何存储，类似java中 “类”的设计
4. 表由列组成，我们也称为字段。所有表都是由一个或多个列组成的，每一列类似java 中的”属性”
5. 表中的数据是按行存储的，每一行类似于java中的“对象”

# 二、 mysql 命令

## 1. 基本命令

- 查看当前所有的数据库：`show databases;`
- 打开指定的库: `use 库名` 
- 查看当前库的所有表: `show tables;` 
- 查看其它库的所有表: `show tables from 库名;` 
- 创建表: `create table 表名(...);` 
- 查看表结构: `desc 表名;` 
- 查看服务器的版本: 
  - 方式一：登录到mysql服务端: `select version();` 
  - 方式二：没有登录到mysql服务端: `mysql --version` 或 `mysql --V` 

## 2. 语法规范

- 不区分大小写,但建议关键字大写，表名、列名小写
- 每条命令最好用分号结尾
- 每条命令根据需要，可以进行缩进 或换行
- 注释： 
  - 单行注释：#注释文字
  - 单行注释：-- 注释文字
  - 多行注释：/* 注释文字  */

## 3. SQL的语言分类

- DQL（Data Query Language）：数据查询语言： `select ` 
- DML(Data Manipulate Language):数据操作语言: `insert 、update、delete` 
- DDL（Data Define Languge）：数据定义语言: `create、drop、alter` 
- TCL（Transaction Control Language）：事务控制语言: `commit、rollback` 

### 1. DQL语言的学习

#### 1. 基础查询

```mysql
SELECT 要查询的东西【FROM 表名】;
```

- 查询表中的**单个字段**： `SELECT last_name FROM employees;` 

- 查询表中的**多个字段**： `SELECT last_name,salary,email FROM employees;` 

- 查询表中的**所有字段**

  - 方式一：

  ```mysql
  SELECT 
      employee_id,
      first_name,
      last_name,
      phone_number,
      last_name,
      job_id,
      phone_number,
      job_id,
      salary,
      commission_pct,
      manager_id,
      department_id,
      hiredate 
  FROM
      employees ;
  ```

  - 方式二：  `SELECT * FROM employees;` 

- 查询常量值

   ```mysql
    SELECT 100;
    SELECT 'john';
   ```

- 查询表达式： `SELECT 100%98;` 

- 查询函数: `SELECT VERSION();` 

- 起别名：

  > 作用：
  >
  > - 便于理解
  > - 如果要查询的字段有重名的情况，使用别名可以区分开来

  - 方式一：**使用as**

  ```mysql
  SELECT 100%98 AS 结果;
  SELECT last_name AS 姓,first_name AS 名 FROM employees;
  ```

  - 方式二：**使用空格**

   `SELECT last_name 姓,first_name 名 FROM employees;`

- 去重： **DISTINCT**

  `SELECT DISTINCT department_id FROM employees;` 

- `+ 号` 的作用： 运算符

  > - 两个操作数都为数值型，则做加法运算： `select 100 + 90; `
  > - 只要其中一方为字符型，试图将字符型数值转换成数值型
  >   - 如果转换成功，则继续做加法运算： `select '123'+90;`
  >   - 如果转换失败，则将字符型数值转换成0 ： `select 'john'+90;	`
  > - 只要其中一方为null，则结果肯定为null :  `select null+10; `

  案例：查询员工名和姓连接成一个字段，并显示为 姓名

  ```mysql
  SELECT CONCAT('a','b','c') AS 结果;
  SELECT CONCAT(last_name,first_name) AS 姓名 FROM employees;
  ```

**特点：**

- 通过select查询完的结果 ，是一个虚拟的表格，不是真实存在
- 要查询的东西可以是:
  - **常量值：** `select 100;` `select 'Jhon';`
  - **表达式：** `select 100 % 98;`
  - **字段：** `select name from user;`
  - **函数：** `select version();`

#### 2. 条件查询

> 根据条件过滤原始表的数据，查询到想要的数据

```mysql
select 
	要查询的字段|表达式|常量值|函数
from 
	表
where 
	条件 ;
```

- 按**条件表达式**筛选

  > 条件运算符： >  <  >=   <=  =   !=   <>

  - 案例1：查询工资 >12000 的员工信息： `SELECT * FROM employees WHERE salary>12000;`
  - 案例2：查询部门编号不等于90号的员工名和部门编号： `SELECT last_name,department_id FROM employees WHERE department_id<>90;`

- 按**逻辑表达式**筛选

  > - and（&&）：两个条件如果同时成立，结果为true，否则为false
  > - or(||)：两个条件只要有一个成立，结果为true，否则为false
  > - not(!)：如果条件成立，则not后为false，否则为true

  - 案例1：查询工资z在10000到20000之间的员工名、工资以及奖金： `SELECT last_name,salary,commission_pct FROM employees WHERE salary>=10000 AND salary<=20000;`
  - 案例2：查询部门编号不是在90到110之间，或者工资高于15000的员工信息：`SELECT * FROM employees WHERE NOT(department_id>=90 AND department_id<=110) OR salary>15000;`

- **模糊查询**

  > - **like**	
  > - **between and**
  > - **in**
  > - **is null | is not null**
  - **like：** 

    > 一般和通配符搭配使用
    > **通配符：**
    >
    > - `%` 任意多个字符,包含0个字符
    > - `_` 任意单个字符

    - 案例1：查询员工名中包含字符a的员工信息： `select * from employees where last_name like '%a%';`

    - 案例2：查询员工名中第三个字符为 n，第五个字符为 l 的员工名和工资： `select last_name,salary FROM employees WHERE last_name LIKE '__n_l%';`

    - 案例3：查询员工名中第二个字符为 _ 的员工名： `SELECT last_name FROM employees WHERE last_name LIKE '_\_%';` 或   `SELECT last_name FROM employees WHERE last_name LIKE '_$_%' ESCAPE '$';`   

      >  `ESCAPE '$'` ： 将  `$`  定义为  `\`  转义字符

  - **between and** ： 

    > - 使用 between and 可以提高语句的简洁度
    > - 包含临界值
    > - 两个临界值不要调换顺序

    - 案例1：查询员工编号在100到120之间的员工信息： `SELECT * FROM employees WHERE employee_id BETWEEN 120 AND 100;`

  - **in：** 

    > **含义：** **判断某字段的值是否属于 in 列表中的某一项**
    > **特点：**
    >
    > - 使用 in 提高语句简洁度
    > - in 列表的值类型必须一致或兼容
    > - in 列表中不支持通配符

    - 案例：查询员工的工种编号是 IT_PROGAD_VP、AD_PRES中的一个员工名和工种编号： `SELECT last_name,job_id FROM employees WHERE job_id IN('IT_PROT','AD_VP','AD_PRES');`

  - **is null：**

    > - = 或 <>不能用于判断null值
    > - is null或is not null 可以判断null值

    - 案例1：查询没有奖金的员工名和奖金率： `SELECT last_name,commission_pct FROM employees WHERE commission_pct IS NULL;`
    - 案例1：查询有奖金的员工名和奖金率： `SELECT last_name,commission_pct FROM employees WHERE commission_pct IS NOT NULL;`

  - **安全等于  <=>**： 

    - 案例1：查询没有奖金的员工名和奖金率： `SELECT last_name,commission_pct FROM employees WHERE commission_pct <=> NULL;`
    - 案例2：查询工资为12000的员工信息： `SELECT last_name,salary FROM employees WHERE salary <=> 12000;`​	

  - **is null  VS  <=>**

    > **IS NULL：**  仅仅可以判断NULL值，可读性较高，建议使用
    > **<=>：**  既可以判断NULL值，又可以判断普通的数值，可读性较低

#### 3. 排序查询	

```mysql
select
	要查询的东西
from
	表
where 
	条件
order by 排序的字段|表达式|函数|别名 【asc|desc】
//asc 代表的是升序，可以省略；desc 代表的是降序
```

- **按单个字段排序**：  `SELECT * FROM employees ORDER BY salary DESC;`

- **添加筛选条件再排序**： 
  - 案例：查询部门编号>=90的员工信息，并按员工编号降序： `SELECT * FROM employees WHERE department_id>=90 ORDER BY employee_id DESC;`

- **按表达式排序：**
  - 案例：查询员工信息 按年薪降序： `SELECT *,salary*12*(1+IFNULL(commission_pct,0)) FROM employees ORDER BY salary*12*(1+IFNULL(commission_pct,0)) DESC;`

- **按别名排序：**
  - 案例：查询员工信息 按年薪升序： `SELECT *,salary*12*(1+IFNULL(commission_pct,0)) 年薪 FROM employees ORDER BY 年薪 ASC;`

- **按函数排序：**
  - 案例：查询员工名，并且按名字的长度降序： `SELECT LENGTH(last_name),last_name FROM employees ORDER BY LENGTH(last_name) DESC;`

- **按多个字段排序：**
  - 案例：查询员工信息，要求先按工资降序，再按employee_id升序： `SELECT * FROM employees ORDER BY salary DESC,employee_id ASC;`

#### 4. 常见函数

1. **单行函数**

   - **字符函数**

     - **concat 拼接**： `SELECT CONCAT(last_name,'_',first_name) 姓名 FROM employees;`
     - **substr 截取子串**： (**索引从1开始**)
       - 截取从指定索引处后面所有字符： `SELECT SUBSTR('李莫愁爱上了陆展元',7)  out_put;`
       - 截取从指定索引处指定字符长度的字符： `SELECT SUBSTR('李莫愁爱上了陆展元',1,3) out_put;`
       - 案例：姓名中首字符大写，其他字符小写然后用_拼接，显示出来： `SELECT CONCAT(UPPER(SUBSTR(last_name,1,1)),'-',LOWER(SUBSTR(last_name,2))) out_put FROM employees;`  
     - **upper 转换成大写**： `SELECT UPPER('john');`
     - **lower 转换成小写**： `SELECT LOWER('joHn');`
     - **trim 消去前后指定的空格和字符：** `SELECT TRIM('aa' FROM 'aaaaaaaaa张aaaaaaaaaaaa翠山')  AS out_put;`
     - **ltrim 消去左边空格**： 
     - **rtrim 消去右边空格**： 
     - **replace 替换**： `SELECT REPLACE('周芷若张无忌爱上了周芷若','周芷若','赵敏') AS out_put;`
     - **lpad 用指定的字符实现左填充指定长度**： `SELECT LPAD('殷素素',2,'*') AS out_put;`
     - **rpad 用指定的字符实现右填充指定长度**： `SELECT RPAD('殷素素',12,'ab') AS out_put;`
     - **instr 返回子串第一次出现的索引，如果找不到返回0**： `SELECT INSTR('杨不殷六侠悔爱上了殷六侠','殷八侠') AS out_put;`   `SELECT LENGTH(TRIM('    张翠山    ')) AS out_put;`
     - **length 获取字节个数** ： `SELECT LENGTH('john');`
       ​	

   - **数学函数**

     - **round 四舍五入** ： `SELECT ROUND(-1.55);`
     - **rand 随机数：** 
     - **floor 向下取整，返回<=该参数的最大整数：** `SELECT FLOOR(-9.99);`
     - **ceil 向上取整,返回 >= 该参数的最小整数**： `SELECT CEIL(-1.02);`
     - **mod 取余：** `SELECT MOD(10,-3);` <==> `SELECT 10%3;`
     - **truncate 截断**： `SELECT TRUNCATE(1.69999,1);`

   - **日期函数**

     - **now 当前系统日期+时间**： `SELECT NOW();`
     - **curdate 当前系统日期，不包含时间**： `SELECT CURDATE();`
     - **curtime 当前系统时间，不包含日期**： `SELECT CURTIME();`

     > 可以获取指定的部分，年、月、日、小时、分钟、秒： 
     >
     > 年：  `SELECT YEAR(NOW());`  `SELECT YEAR('1998-1-1');`
     >
     > 月： `SELECT MONTH(NOW());`  `SELECT MONTHNAME(NOW());`

     - **str_to_date 将字符转换成日期**： `SELECT STR_TO_DATE('1998-3-2','%Y-%c-%d') AS out_put;`
     - **date_format 将日期转换成字符**： `SELECT DATE_FORMAT(NOW(),'%y年%m月%d日') AS out_put;`

   - **流程控制函数**

     - **if 处理双分支**： `SELECT IF(10<5,'大','小');`  

     - **case语句 处理多分支**： 

       - **处理等值判断，switch case 的效果**： 

         ```mysql
         SELECT salary 原始工资,department_id,
         CASE department_id
         WHEN 30 THEN salary*1.1
         WHEN 40 THEN salary*1.2
         WHEN 50 THEN salary*1.3
         ELSE salary
         END 
         AS 新工资
         FROM employees;
         ```

       - **处理条件判断，类似于多重 if**： 

         ```	mysql
         SELECT salary,
         CASE 
         WHEN salary>20000 THEN 'A'
         WHEN salary>15000 THEN 'B'
         WHEN salary>10000 THEN 'C'
         ELSE 'D'
         END 
         AS 工资级别
         FROM employees;
         ```

   - **其他函数**

     - **version 版本**： `SELECT VERSION();`
     - **database 当前库**： `SELECT DATABASE();`
     - **user 当前连接用户**： `SELECT USER();`

2. **分组函数**

   - **sum 求和**
   - **max 最大值**
   - **min 最小值**
   - **avg 平均值**
   - **count 计数**

   > **特点：**
   >
   > - 以上五个分组函数都忽略 null 值，除了count(*)*
   > - sum 和 avg 一般用于处理数值型，max、min、count可以处理任何数据类型
   > - 都可以搭配 distinct 使用，用于统计去重后的结果
   > - count的参数可以支持：字段、常量值

#### 5. 分组查询

> **特点：**
>
> - 可以按单个字段分组
>
> - 和分组函数一同查询的字段最好是分组后的字段
>
>   分组筛选              针对的表	                 位置		       关键字
>   分组前筛选：	       原始表		group by的前面		**where**
>   分组后筛选：	分组后的结果集	group by的后面		**having**
>
> - 可以按多个字段分组，字段之间用逗号隔开
> - 可以支持排序
> - having后可以支持别名

```mysql
select 查询的字段，分组函数
from 表
group by 分组的字段
【where 筛选条件】
group by 分组的字段
【order by 排序的字段】;
```

- **简单的分组**
  - 案例1：查询每个工种的员工平均工资： `SELECT AVG(salary),job_id FROM employees GROUP BY job_id;`
  - 案例2：查询每个位置的部门个数： `SELECT COUNT(*),location_id FROM departments GROUP BY location_id;`

- **分组前筛选**
  - 案例1：查询邮箱中包含 a 字符的每个部门的最高工资： `SELECT MAX(salary),department_id FROM employees WHERE email LIKE '%a%' GROUP BY department_id;`
  - 案例2：查询有奖金的每个领导手下员工的平均工资： `SELECT AVG(salary),manager_id FROM employees WHERE commission_pct IS NOT NULL GROUP BY manager_id;`

- **分组后筛选**
  - 案例1：查询哪个部门的员工个数 >5 ： 
    - ①查询每个部门的员工个数： `SELECT COUNT(*),department_id FROM employees GROUP BY department_id;`
    - ② 筛选刚才①结果： `SELECT COUNT(*),department_id FROM employees GROUP BY department_id HAVING COUNT(*)>5;`
  - 案例2：每个工种有奖金的员工的最高工资>12000的工种编号和最高工资： `SELECT job_id,MAX(salary) FROM employees WHERE commission_pct IS NOT NULL GROUP BY job_id HAVING MAX(salary)>12000;`
  - 案例3：领导编号>102的每个领导手下的最低工资大于5000的领导编号和最低工资： `SELECT manager_id,MIN(salary) FROM employees GROUP BY manager_id HAVING MIN(salary)>5000;`

- **添加排序**
  - 案例：每个工种有奖金的员工的最高工资>6000的工种编号和最高工资,按最高工资升序： `SELECT job_id,MAX(salary) m FROM employees WHERE commission_pct IS NOT NULL GROUP BY job_id HAVING m>6000 ORDER BY m ;`

- **按多个字段分组**
  - 案例：查询每个工种每个部门的最低工资,并按最低工资降序： `SELECT MIN(salary),job_id,department_id FROM employees GROUP BY department_id,job_id ORDER BY MIN(salary) DESC;`

#### 6. 多表连接查询

1. 传统模式下的连接**(sql92标准) ：等值连接——非等值连接**

   > 1. 等值连接的结果 = 多个表的交集
   > 2. n 表连接，至少需要 n-1个连接条件
   > 3. 多个表不分主次，没有顺序要求
   > 4. 一般为表起别名，提高阅读性和性能

- **等值连接**

  - 案例1：查询女神名和对应的男神名： `SELECT NAME,boyName FROM boys,beauty WHERE beauty.boyfriend_id= boys.id;`

  - 案例2：查询员工名和对应的部门名： `SELECT last_name,department_name FROM employees,departments WHERE employees.department_id=departments.department_id;`

  Ⅰ. **为表起别名**(两个表的顺序可以调换)

  > - 提高语句的简洁度
  > - 区分多个重名的字段
  >
  > **注意：** 如果为表起了别名，则查询的字段就不能使用原来的表名去限定

  - 查询员工名、工种号、工种名： `SELECT e.last_name,e.job_id,j.job_title FROM employees e,jobs j WHERE e.job_id=j.job_id;`

  - 两个表的顺序调换： `SELECT e.last_name,e.job_id,j.job_title FROM jobs j,employees e WHERE e.job_id=j.job_id;`

  Ⅱ. **可以加筛选**

  - 案例1：查询有奖金的员工名、部门名： `SELECT last_name,department_name,commission_pct FROM employees e,departments d WHERE e.department_id=d.department_id AND e.commission_pct IS NOT NULL;`
  - 案例2：查询城市名中第二个字符为o的部门名和城市名： `SELECT department_name,city FROM departments d,locations l WHERE d.location_id = l.location_id AND city LIKE '_o%';` 

  **III. 可以加分组**

  - 案例1：查询每个城市的部门个数： `SELECT COUNT(*) 个数,city FROM departments d,locations l WHERE d.location_id=l.location_id GROUP BY city;`

  - 案例2：查询有奖金的每个部门的部门名和部门的领导编号和该部门的最低工资：

    ```mysql
    SELECT department_name,d.manager_id,MIN(salary) 
    FROM departments d,employees e 
    WHERE d.department_id=e.department_id AND commission_pct IS NOT NULL 
    GROUP BY department_name,d.manager_id;
    ```

  Ⅳ. **可以加排序**

  - 案例：查询每个工种的工种名和员工的个数，并且按员工个数降序

    ```mysql
    SELECT job_title,COUNT(*)
    FROM employees e,jobs j
    WHERE e.job_id=j.job_id
    GROUP BY job_title
    ORDER BY COUNT(*) DESC;
    ```

  V. **实现三表连接**

  - 案例：查询员工名、部门名和所在的城市

    ```mysql
    SELECT last_name,department_name,city
    FROM employees e,departments d,locations l
    WHERE e.department_id = d.department_id
    AND d.location_id = l.location_id
    AND city LIKE 's%'
    ORDER BY department_name DESC;
    ```

  - **非等值连接**

    - 案例1：查询员工的工资和工资级别

      ```mysql
      SELECT salary,grade_level
      FROM employees e,job_grades g
      WHERE salary BETWEEN g.lowest_sal AND g.highest_sal
      AND g.grade_level='A';
      ```

- **自连接**

  - 案例：查询 员工名和上级的名称： 

    ```mysql
    SELECT e.employee_id,e.last_name,m.employee_id,m.last_name
    FROM employees e,employees m
    WHERE e.manager_id=m.employee_id;
    ```

2. **sql99语法**：通过join关键字实现连接

   > **支持：**
   >
   > - 等值连接、非等值连接 （内连接：**inner**）
   > - 外连接：左外：**left 【outer】**，右外：**right 【outer】**，全外：**full【outer】**
   > - 交叉连接：**cross** 

   ```mysql
   select 字段，...
   from 表1
   【inner|left outer|right outer|cross】join 表2 on  连接条件
   【inner|left outer|right outer|cross】join 表3 on  连接条件
   【where 筛选条件】
   【group by 分组字段】
   【having 分组后的筛选条件】
   【order by 排序的字段或表达式】
   ```

- **内连接**

  > **分类：** 等值、非等值、自连接
  >
  > **特点：** 
  >
  > - 添加排序、分组、筛选
  > - **inner 可以省略**
  > - 筛选条件放在 where 后面，连接条件放在 on 后面，提高分离性，便于阅读
  > - inner join 连接和sql92语法中的等值连接效果是一样的，都是查询多表的交集

  ```mysql
  select 查询列表
  from 表1 别名
  inner join 表2 别名
  on 连接条件;
  ```

  - **等值连接**

    - 案例1. 查询员工名、部门名

      ```mysql
      SELECT last_name,department_name
      FROM departments d
      JOIN employees e
      ON e.department_id = d.department_id;
      ```

    - 案例2. 查询名字中包含e的员工名和工种名（添加筛选）

      ```mysql
      SELECT last_name,job_title
      FROM employees e
      INNER JOIN jobs j
      ON e.job_id=  j.job_id
      WHERE e.last_name LIKE '%e%';
      ```

    - 案例3. 查询部门个数>3的城市名和部门个数（添加分组+筛选）

      ```mysql
      SELECT city,COUNT(*) 部门个数
      FROM departments d
      INNER JOIN locations l
      ON d.location_id=l.location_id
      GROUP BY city
      HAVING COUNT(*)>3;
      ```

    - 案例4. 查询哪个部门的员工个数>3的部门名和员工个数，并按个数降序（添加排序）

      ①查询每个部门的员工个数

      ```mysql
      SELECT COUNT(*),department_name
      FROM employees e
      INNER JOIN departments d
      ON e.department_id=d.department_id
      GROUP BY department_name
      ```

      ② 在①结果上筛选员工个数>3的记录，并排序

      ```mysql
      SELECT COUNT(*) 个数,department_name
      FROM employees e
      INNER JOIN departments d
      ON e.department_id=d.department_id
      GROUP BY department_name
      HAVING COUNT(*)>3
      ORDER BY COUNT(*) DESC;
      ```

    - 案例5. 查询员工名、部门名、工种名，并按部门名降序（添加三表连接）

      ```mysql
      SELECT last_name,department_name,job_title
      FROM employees e
      INNER JOIN departments d ON e.department_id=d.department_id
      INNER JOIN jobs j ON e.job_id = j.job_id
      ORDER BY department_name DESC;
      ```

  - **非等值连接**

    - 查询员工的工资级别

      ```mysql
      SELECT salary,grade_level
      FROM employees e
      JOIN job_grades g
      ON e.salary BETWEEN g.lowest_sal AND g.highest_sal;
      ```

    - 查询工资级别的个数>20的个数，并且按工资级别降序

       ```mysql
        SELECT COUNT(),grade_level
        FROM employees e
        JOIN job_grades g ON e.salary BETWEEN g.lowest_sal AND g.highest_sal
        GROUP BY grade_level
        HAVING COUNT(*)>20
        ORDER BY grade_level DESC;
       ```

  - **自连接**

    - 查询员工的名字、上级的名字

      ```mysql
      SELECT e.last_name,m.last_name 
      FROM employees e
      JOIN employees m ON e.manager_id= m.employee_id;
      ```

    - 查询姓名中包含字符k的员工的名字、上级的名字

      ```mysql
      SELECT e.last_name,m.last_name
      FROM employees e
      JOIN employees m ON e.manager_id= m.employee_id
      WHERE e.last_name LIKE '%k%';
      ```

- **外连接**

  > **应用场景：** 用于查询一个表中有，另一个表没有的记录
  >
  >  **特点：**
  >
  > - 外连接的查询结果为主表中的所有记录
  >   如果从表中有和它匹配的，则显示匹配的值
  >   如果从表中没有和它匹配的，则显示null
  >   外连接查询结果=内连接结果+主表中有而从表没有的记录
  > - 左外连接，left join 左边的是主表
  >   右外连接，right join 右边的是主表
  > - 左外和右外交换两个表的顺序，可以实现同样的效果 
  > - 全外连接=内连接的结果+表1中有但表2没有的+表2中有但表1没有的
  - 案例：查询哪个部门没有员工

    - **左外**

      ```mysql
      SELECT d.*,e.employee_id
      FROM departments d
      LEFT OUTER JOIN employees e
      ON d.department_id = e.department_id
      WHERE e.employee_id IS NULL;
      ```

    - **右外**

      ```mysql
      SELECT d.*,e.employee_id
      FROM employees e
      RIGHT OUTER JOIN departments d
      ON d.department_id = e.department_id
      WHERE e.employee_id IS NULL;
      ```

    - **全外**

      ```mysql
      SELECT b.,bo.
      FROM beauty b
      FULL OUTER JOIN boys bo
      ON b.boyfriend_id = bo.id;
      ```

- **交叉连接**

  ```mysql
  SELECT b.*,bo.*
  FROM beauty b
  CROSS JOIN boys bo;
  ```

#### 7. 子查询

> **含义：**一条查询语句中又嵌套了另一条完整的select语句，其中被嵌套的select语句，称为子查询或内查询，在外面的查询语句，称为主查询或外查询
>
> **特点：**
>
> - 子查询都放在小括号内
> - 子查询可以放在 from 后面、select 后面、where 后面、having 后面，但一般放在条件的右侧
> - 子查询优先于主查询执行，主查询使用了子查询的执行结果
>
> **分类：**
>
> - 按子查询出现的位置：
>   - **select后面：** 仅仅支持标量子查询
>   - **from后面：** 支持表子查询
>   - **where或having后面**：
>     - 标量子查询（单行）
>     - 列子查询（多行）	
>     - 行子查询
>   - **exists后面（相关子查询）**： 表子查询
>
> - 按结果集的行列数不同：
>   - 标量子查询（结果集只有一行一列）
>   - 列子查询（结果集只有一列多行）
>   - 行子查询（结果集有一行多列）
>   - 表子查询（结果集一般为多行多列）



**一、where或having后面**

> - 标量子查询（单行子查询）
> - 列子查询（多行子查询）
> - 行子查询（多列多行）
>
> **特点：** 
>
> - 子查询放在小括号内
> - 子查询一般放在条件的右侧
>   - 标量子查询，一般搭配着单行操作符使用： < >= <= = <>
>   - 列子查询，一般搭配着多行操作符使用： in、any/some、all
>
> - **子查询的执行优先于主查询执行，主查询的条件用到了子查询的结果**

- **标量子查询**
  - 案例1：谁的工资比 Abel 高?

    ```mysql
    SELECT *
    FROM employees
    WHERE salary > (
    	SELECT salary
    	FROM employees
    	WHERE last_name = 'Abel'
    );
    ```

  - 案例2：返回job_id与141号员工相同，salary比143号员工多的员工 姓名，job_id 和工资

    ```mysql
    SELECT last_name,job_id,salary
    FROM employees
    WHERE job_id = (
    	SELECT job_id
    	FROM employees
    	WHERE employee_id = 141
    ) AND salary>(
    	SELECT salary
    	FROM employees
    	WHERE employee_id = 143
    );
    ```

  - 案例3：返回公司工资最少的员工的last_name,job_id和salary

    ```mysql
    SELECT last_name,job_id,salary
    FROM employees
    WHERE salary=(
    	SELECT MIN(salary)
    	FROM employees
    );
    ```

  - 案例4：查询最低工资大于50号部门最低工资的部门id和其最低工资

    ```mysql
    SELECT MIN(salary),department_id
    FROM employees
    GROUP BY department_id
    HAVING MIN(salary)>(
    	SELECT MIN(salary)
    	FROM employees
    	WHERE department_id = 50
    );
    # 非法使用标量子查询
    SELECT MIN(salary),department_id
    FROM employees
    GROUP BY department_id
    HAVING MIN(salary)>(
    	SELECT salary #***
    	FROM employees
    	WHERE department_id = 250
    );
    ```

- **列子查询（多行子查询）**
  - 案例1：返回location_id是1400或1700的部门中的所有员工姓名

    ```mysql
    SELECT last_name
    FROM employees
    WHERE department_id  <>ALL(
    	SELECT DISTINCT department_id
    	FROM departments
    	WHERE location_id IN(1400,1700)
    );
    ```

  - 案例2：返回其它工种中比job_id为‘IT_PROG’工种任一工资低的员工的员工号、姓名、job_id 以及salary

    ```mysql
    SELECT last_name,employee_id,job_id,salary
    FROM employees
    WHERE salary<ANY(
    	SELECT DISTINCT salary
    	FROM employees
    	WHERE job_id = 'IT_PROG'
    ) AND job_id<>'IT_PROG';
    
    或
    
    SELECT last_name,employee_id,job_id,salary
    FROM employees
    WHERE salary<(
    	SELECT MAX(salary)
    	FROM employees
    	WHERE job_id = 'IT_PROG'
    ) AND job_id<>'IT_PROG';
    ```

  - 案例3：返回其它部门中比job_id为‘IT_PROG’部门所有工资都低的员工   的员工号、姓名、job_id 以及salary

    ```mysql
    SELECT last_name,employee_id,job_id,salary
    FROM employees
    WHERE salary<ALL(
    	SELECT DISTINCT salary
    	FROM employees
    	WHERE job_id = 'IT_PROG'
    ) AND job_id<>'IT_PROG';
    
    或
    
    SELECT last_name,employee_id,job_id,salary
    FROM employees
    WHERE salary<(
    	SELECT MIN( salary)
    	FROM employees
    	WHERE job_id = 'IT_PROG'
    ) AND job_id<>'IT_PROG';
    ```

- **行子查询（结果集一行多列或多行多列）**
  - 案例：查询员工编号最小并且工资最高的员工信息

    ```mysql
    SELECT * 
    FROM employees
    WHERE (employee_id,salary)=(
    	SELECT MIN(employee_id),MAX(salary)
    	FROM employees
    );
    
    或
    
    SELECT *
    FROM employees
    WHERE employee_id=(
    	SELECT MIN(employee_id)
    	FROM employees
    )AND salary=(
    	SELECT MAX(salary)
    	FROM employees
    );
    ```

**二、select后面**

> 仅仅支持标量子查询

- 案例：查询每个部门的员工个数

  ```mysql
  SELECT d.*,(
  	SELECT COUNT(*)
  	FROM employees e
  	WHERE e.department_id = d.department_id
   ) 个数
   FROM departments d;
  ```

- 案例2：查询员工号=102的部门名

  ```mysql
  SELECT (
  	SELECT department_name,e.department_id
  	FROM departments d
  	INNER JOIN employees e
  	ON d.department_id=e.department_id
  	WHERE e.employee_id=102
  ) 部门名;
  ```

**三、from后面**

> 将子查询结果充当一张表，要求必须起别名

- 案例：查询每个部门的平均工资的工资等级

  ```mysql
  SELECT  ag_dep.*,g.grade_level
  FROM (
  	SELECT AVG(salary) ag,department_id
  	FROM employees
  	GROUP BY department_id
  ) ag_dep
  INNER JOIN job_grades g
  ON ag_dep.ag BETWEEN lowest_sal AND highest_sal;
  ```

**四、exists后面（相关子查询）**

- 案例1：查询有员工的部门名

  ```mysql
  #in
  SELECT department_name
  FROM departments d
  WHERE d.department_id IN(
  	SELECT department_id
  	FROM employees
  )
  
  #exists
  SELECT department_name
  FROM departments d
  WHERE EXISTS(
  	SELECT *
  	FROM employees e
  	WHERE d.department_id=e.department_id
  );
  ```

- 案例2：查询没有女朋友的男神信息

  ```mysql
  #in
  SELECT bo.*
  FROM boys bo
  WHERE bo.id NOT IN(
  	SELECT boyfriend_id
  	FROM beauty
  )
  
  #exists
  SELECT bo.*
  FROM boys bo
  WHERE NOT EXISTS(
  	SELECT boyfriend_id
  	FROM beauty b
  	WHERE bo.id=b.boyfriend_id
  );
  ```

#### 8. 分页查询

> **应用场景：** 实际的web项目中需要根据用户的需求提交对应的分页查询的sql语句
>
> **语法：** 
>
> ```mysql
> select 查询列表
> from 表
> 	【join type join 表2
> 	on 连接条件
> 	where 筛选条件
> 	group by 分组字段
> 	having 分组后的筛选
> 	order by 排序的字段】
> limit 【offset,】size;
> # offset要显示条目的起始索引（起始索引从0开始）
> # size 要显示的条目个数
> ```
>
> **特点：** limit 语句放在查询语句的最后

- 案例1：查询前五条员工信息

  ```mysql
  SELECT * FROM  employees LIMIT 0,5;
  SELECT * FROM  employees LIMIT 5;
  ```

- 案例2：查询第11条——第25条

  ```mysql
  SELECT * FROM  employees LIMIT 10,15;
  ```

- 案例3：有奖金的员工信息，并且工资较高的前10名显示出来

  ```mysql
  SELECT *
  FROM employees 
  WHERE commission_pct IS NOT NULL 
  ORDER BY salary DESC 
  LIMIT 10 ;
  ```

#### 9. 联合查询

> **union 联合 合并：** 将多条查询语句的结果合并成一个结果
>
> **语法：** 
>
> ```mysql
> 语法：
> 查询语句1
> union
> 查询语句2
> union
> ```
>
> **应用场景：** 要查询的结果来自于多个表，且多个表没有直接的连接关系，但查询的信息一致时
>
> **特点：** 
>
> - 要求多条查询语句的查询列数是一致的！
> - 要求多条查询语句的查询的每一列的类型和顺序最好一致
> - union关键字默认去重，如果使用union all 可以包含重复项

- 引入的案例：查询部门编号>90或邮箱包含a的员工信息

  ```mysql
  SELECT * FROM employees WHERE email LIKE '%a%' OR department_id>90;
  
  SELECT * FROM employees  WHERE email LIKE '%a%'
  UNION
  SELECT * FROM employees  WHERE department_id>90;
  ```

- 案例：查询中国用户中男性的信息以及外国用户中年男性的用户信息

  ```mysql
  SELECT id,cname FROM t_ca WHERE csex='男'
  UNION ALL
  SELECT t_id,tname FROM t_ua WHERE tGender='male';
  ```

### 2. DML语言

#### 1. 插入

1. **经典的插入**： 

   ```mysql
   insert into 表名(字段名，...) values(值1，...);
   ```

   **特点：** 

   - 字段类型和值类型一致或兼容，而且一一对应

     ```mysql
     INSERT INTO beauty(id,NAME,sex,borndate,phone,photo,boyfriend_id)
     VALUES(13,'唐艺昕','女','1990-4-23','1898888888',NULL,2);
     ```

   - 可以为空的字段，可以不用插入值，或用null填充

     ```mysql
     INSERT INTO beauty(id,NAME,sex,borndate,phone,photo,boyfriend_id)
     VALUES(13,'唐艺昕','女','1990-4-23','1898888888',NULL,2);
     ```

   - 不可以为空的字段，必须插入值

   - 字段个数和值的个数必须一致

   - 字段可以省略，但默认所有字段，并且顺序和表中的存储顺序一致

     ```mysql
     INSERT INTO beauty
     VALUES(18,'张飞','男',NULL,'119',NULL,NULL);
     ```

2. **新方式**

   ```mysql
   insert into 表名 set 列名=值,列名=值,...
   ```

   例子：

   ```mysql
   INSERT INTO beauty SET id=19,NAME='刘涛',phone='999';
   ```

3. 两种方式比较

   - **方式一支持插入多行,方式二不支持**

     ```mysql
     INSERT INTO beauty
     VALUES(23,'唐艺昕1','女','1990-4-23','1898888888',NULL,2),
     (24,'唐艺昕2','女','1990-4-23','1898888888',NULL,2),
     (25,'唐艺昕3','女','1990-4-23','1898888888',NULL,2);
     ```

   - **方式一支持子查询，方式二不支持**

#### 2. 修改

- **修改单表语法：**

  ```mysql
  update 表名 set 字段=新值,字段=新值 【where 条件】
  ```

  - 案例1：修改beauty表中姓唐的女神的电话为13899888899

    ```mysql
    UPDATE beauty SET phone = '13899888899' WHERE NAME LIKE '唐%';
    ```

  - 案例2：修改boys表中id好为2的名称为张飞，魅力值 10

    ```mysql
    UPDATE boys SET boyname='张飞',usercp=10 WHERE id=2;
    ```

- **修改多表语法：**

  ```mysql
  # sql92语法：
  update 表1 别名,表2 别名
  set 列=值,...
  where 连接条件
  and 筛选条件;
  
  # sql99语法：
  update 表1 别名
  inner|left|right join 表2 别名
  on 连接条件
  set 列=值,...
  where 筛选条件;
  ```

  - 案例 1：修改张无忌的女朋友的手机号为114

    ```mysql
    UPDATE boys bo
    INNER JOIN beauty b ON bo.id=b.boyfriend_id
    SET b.phone='119',bo.userCP=1000
    WHERE bo.boyName='张无忌';
    ```

  - 案例2：修改没有男朋友的女神的男朋友编号都为2号

    ```mysql
    UPDATE boys bo
    RIGHT JOIN beauty b ON bo.id=b.boyfriend_id
    SET b.boyfriend_id=2
    WHERE bo.id IS NULL;
    ```

#### 3. 删除

- **delete语句** 
  - 单表的删除： ★

    ```mysql
    delete from 表名 【where 筛选条件】
    ```

    - 案例：删除手机号以9结尾的女神信息

      ```mysql
      DELETE FROM beauty WHERE phone LIKE '%9';
      ```

  - 多表的删除：

    ```mysql
    # sql92语法：
    delete 表1的别名,表2的别名
    from 表1 别名,表2 别名
    where 连接条件
    and 筛选条件;
    
    # sql99语法：
    delete 表1的别名,表2的别名
    from 表1 别名
    inner|left|right join 表2 别名 on 连接条件
    where 筛选条件;
    ```

    - 案例：删除张无忌的女朋友的信息

      ```mysql
      DELETE b
      FROM beauty b
      INNER JOIN boys bo ON b.boyfriend_id = bo.id
      WHERE bo.boyName='张无忌';
      ```

    - 案例：删除黄晓明的信息以及他女朋友的信息

      ```mysql
      DELETE b,bo
      FROM beauty b
      INNER JOIN boys bo ON b.boyfriend_id=bo.id
      WHERE bo.boyName='黄晓明';
      ```

- **truncate语句**

  ```mysql
  truncate table 表名
  ```

  - 案例：将魅力值>100的男神信息删除

    ```mysql
    TRUNCATE TABLE boys;
    ```

- **delete VS truncate** 

  > - delete 可以加where 条件，truncate不能加
  >
  > - truncate 删除，效率高一丢丢
  > - 假如要删除的表中有自增长列，如果用delete删除后，再插入数据，自增长列的值从断点开始，而truncate删除后，再插入数据，自增长列的值从1开始。
  > - truncate删除没有返回值，delete删除有返回值
  > - truncate删除不能回滚，delete删除可以回滚.

### 3. DDL语句

#### 1. 库和表的管理

**库的管理：**

- **创建库**

  ```mysql
  create database  [if not exists]库名;
  ```

  - 案例：创建库Books

    ```mysql
    CREATE DATABASE IF NOT EXISTS books ;
    ```

- **库的修改**

  ```mysql
  RENAME DATABASE books TO 新库名;
  ```

  - 更改库的字符集

    ```mysql
    ALTER DATABASE books CHARACTER SET gbk;
    ```

- **库的删除** 

  ```mysql
  DROP DATABASE IF EXISTS books;
  ```

**表的管理：**

- **创建表**	

  ```mysql
  create table 表名(
  	列名 列的类型【(长度) 约束】,
  	列名 列的类型【(长度) 约束】,
  	列名 列的类型【(长度) 约束】,
  	...
  	列名 列的类型【(长度) 约束】
  )
  ```

  - 案例：创建表Book

    ```mysql
    CREATE TABLE IF NOT EXISTS book(
    	id INT,#编号
    	bName VARCHAR(20),#图书名
    	price DOUBLE,#价格
    	authorId  INT,#作者编号
    	publishDate DATETIME#出版日期
    );
    ```

- **修改表 alter** 

  ```mysql
  ALTER TABLE 表名 ADD|MODIFY|DROP|CHANGE COLUMN 字段名 【字段类型】;
  ```

  - 修改字段名

    ```mysql
    ALTER TABLE studentinfo CHANGE COLUMN sex gender CHAR;
    ```

  - 修改表名

    ```mysql
    ALTER TABLE stuinfo RENAME [TO]  studentinfo;
    ```

  - 修改字段类型和列级约束

    ```mysql
    ALTER TABLE studentinfo MODIFY COLUMN borndate DATE ;
    ```

  - 添加字段

    ```mysql
    ALTER TABLE studentinfo ADD COLUMN email VARCHAR(20) first;
    ```

  - 删除字段

    ```mysql
    ALTER TABLE studentinfo DROP COLUMN email;
    ```

- **删除表**

  ```mysql
  DROP TABLE [IF EXISTS] studentinfo;
  ```

- **表的复制**

  - 仅仅复制表的结构

    ```mysql
    CREATE TABLE copyTmp LIKE author;
    ```

  - 复制表的结构+数据

    ```mysql
    CREATE TABLE copyTmp2 SELECT * FROM author;
    ```

  - 只复制部分数据

    ```mysql
    CREATE TABLE copyTmp3
    SELECT id,au_name
    FROM author 
    WHERE nation='中国';
    ```

  - 仅仅复制某些字段

    ```mysql
    CREATE TABLE copy4 
    SELECT id,au_name
    FROM author
    WHERE 0;
    ```

#### 2. 常见类型

> - **数值型：**
>   - **整型**
>   - **小数：**
>     - **定点数**
>     - **浮点数**
> - **字符型：**
>   - **较短的文本：** char、varchar
>   - **较长的文本：** text、blob（较长的二进制数据）
>
> - **日期型：**

**一、整型**

> **分类：**
>
> - tinyint：1
>
> - smallint： 2
>
> - mediumint： 3
> - int/integer： 4
> - bigint： 8
>
> **特点：**
>
> - 如果不设置无符号还是有符号，**默认是有符号**，如果想设置无符号，需要添加 unsigned 关键字
> - 如果插入的数值超出了**整型的范围**,会报out of range异常，并且插入临界值
> - 如果不设置长度，会**有默认的长度**，长度代表了显示的最大宽度，如果不够会用 0 在左边填充，但必须搭配 `zerofill` 使用

**二、小数**

> **原则**： 所选择的类型越简单越好，能保存数值的类型越小越好
>
> **分类：**
>
> - 浮点型： `float(M,D) double(M,D)` 
> - 定点型： `dec(M，D) decimal(M,D)` 
>
> **特点：**
>
> - M：整数部位+小数部位
>   D：小数部位
>   如果超过范围，则插入临界值
>
> - M和D都可以省略
>   如果是decimal，则M默认为10，D默认为0
>   如果是float和double，则会根据插入的数值的精度来决定精度
>
> - 定点型的精确度较高，如果要求插入数值的精度较高如货币运算等则考虑使用

**三、字符型** 

> - **较短的文本**： char、varchar
>
> - **其他：**
>   - binary 和 varbinary 用于保存较短的二进制
>   - enum 用于保存枚举
>   - set 用于保存集合
>
> - **较长的文本**： text、blob(较大的二进制)
>
> **特点：**
>
>   写法		M的意思					特点			   		空间的耗费			   效率
>
>   char		char(M)		最大的字符数，可以省略，默认为1         固定长度的字符		比较耗费高
>
> varchar 	      varchar(M)	    最大的字符数，不可以省略		     可变长度的字符		比较节省低

**四、日期型** 

> **分类：**
>
> - date  只保存日期
> - time 只保存时间
> - year  只保存年
>
> - datetime   保存日期+时间
> - timestamp  保存日期+时间
>
> **特点：**
>
>   			     字节		       范围				时区等的影响
>
> datetime	       8		1000——9999	                  不受
> timestamp	       4	        1970——2038	                    受

#### 3. 常见约束

> **含义：** 一种限制，用于限制表中的数据，为了保证表中的数据的准确和可靠性
>
> **分类：六大约束**
>
> - **NOT NULL**：非空，用于保证该字段的值不能为空
> - **DEFAULT**： 默认，用于保证该字段有默认值
> - **PRIMARY KEY**： 主键，用于保证该字段的值具有唯一性，并且非空
> - **UNIQUE**： 唯一，用于保证该字段的值具有唯一性，可以为空
> - **CHECK**： 检查约束【mysql中不支持】
> - **FOREIGN KEY**： 外键，用于限制两个表的关系，用于保证该字段的值必须来自于主表的关联列的值，在从表添加外键约束，用于引用主表中某列的值	
>
>
>
> **添加约束的时机：**
>
> - 创建表时
> - 修改表时
>   ​	
>
> **约束的添加分类：**
>
> - **列级约束**：六大约束语法上都支持，但外键约束没有效果
> - **表级约束**：除了非空、默认，其他的都支持			
>
>
>
> **主键和唯一的大对比：** 
>
> ​			保证唯一性    是否允许为空       一个表中可以有多少个      	是否允许组合
>
> ​     主键	       		√			×				至多有1个         	  	 √，但不推荐
> ​     唯一			√			√				可以有多个         		 √，但不推荐
>
>
>
> **外键：**
>
> - 要求在**从表设置**外键关系
> - 从表的外键列的类型和主表的关联列的类型要求一致或兼容，名称无要求
> - 主表的关联列必须是一个key（一般是主键或唯一）
> - 插入数据时，先插入主表，再插入从表
>   删除数据时，先删除从表，再删除主表

**一、创建表时添加约束**

```mysql
CREATE TABLE 表名(
	字段名 字段类型 列级约束,
	字段名 字段类型,表级约束
)
```

- **添加列级约束**

  > - 直接在字段名和类型后面追加 约束类型即可
  >
  > - 只支持：默认、非空、主键、唯一

  ```mysql
  CREATE TABLE stuinfo(
  	id INT PRIMARY KEY,#主键
  	stuName VARCHAR(20) NOT NULL UNIQUE,#非空
  	gender CHAR(1) CHECK(gender='男' OR gender ='女'),#检查
  	seat INT UNIQUE,#唯一
  	age INT DEFAULT  18,#默认约束
  	majorId INT REFERENCES major(id)#外键
  );
  CREATE TABLE major(
  	id INT PRIMARY KEY,
  	majorName VARCHAR(20)
  );
  
  # 查看stuinfo中的所有索引，包括主键、外键、唯一
  SHOW INDEX FROM stuinfo;
  ```

- **添加表级约束**

  > 语法：在各个字段的最下面
  > 【constraint 约束名】 约束类型(字段名) 

  ```mysql
  CREATE TABLE stuinfo(
  	id INT,
  	stuname VARCHAR(20),
  	gender CHAR(1),
  	seat INT,
  	age INT,
  	majorid INT,
  	CONSTRAINT pk PRIMARY KEY(id),#主键
  	CONSTRAINT uq UNIQUE(seat),#唯一键
  	CONSTRAINT ck CHECK(gender ='男' OR gender  = '女'),#检查
  	CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorid) REFERENCES major(id)#外键
  );
  或
  CREATE TABLE IF NOT EXISTS stuinfo(
  	id INT PRIMARY KEY,
  	stuname VARCHAR(20),
  	sex CHAR(1),
  	age INT DEFAULT 18,
  	seat INT UNIQUE,
  	majorid INT,
  	CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorid) REFERENCES major(id)
  );
  
  
  SHOW INDEX FROM stuinfo;
  ```

**二、修改表时添加约束**

> - 添加列级约束： `alter table 表名 modify column 字段名 字段类型 新约束;`
> - 添加表级约束： `alter table 表名 add 【constraint 约束名】 约束类型(字段名) 【外键的引用】;`

- 添加非空约束

  ```mysql
  ALTER TABLE stuinfo MODIFY COLUMN stuname VARCHAR(20)  NOT NULL;
  ```

- 添加默认约束

  ````mysql
  ALTER TABLE stuinfo MODIFY COLUMN age INT DEFAULT 18;
  ````

- 添加主键
  - 列级约束

    ```mysql
    ALTER TABLE stuinfo MODIFY COLUMN id INT PRIMARY KEY;
    ```

  - 表级约束

    ```mysql
    ALTER TABLE stuinfo ADD PRIMARY KEY(id);
    ```

- 添加唯一
  - 列级约束

    ```mysql
    ALTER TABLE stuinfo MODIFY COLUMN seat INT UNIQUE;
    ```

  - 表级约束

    ```mysql
    ALTER TABLE stuinfo ADD UNIQUE(seat);
    ```

- 添加外键

  ```mysql
  ALTER TABLE stuinfo ADD CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorid) REFERENCES major(id); 
  ```

**三、修改表时删除约束**

- 删除非空约束

  ```mysql
  ALTER TABLE stuinfo MODIFY COLUMN stuname VARCHAR(20) NULL;
  ```

- 删除默认约束

  ```mysql
  ALTER TABLE stuinfo MODIFY COLUMN age INT ;
  ```

- 删除主键

  ```mysql
  ALTER TABLE stuinfo DROP PRIMARY KEY;
  ```

- 删除唯一

  ```mysql
  ALTER TABLE stuinfo DROP INDEX seat;
  ```

- 删除外键

  ```mysql
  ALTER TABLE stuinfo DROP FOREIGN KEY fk_stuinfo_major;
  ```

# 三、mysql 进阶

## 1. 数据库事务(TCL)

> **含义**： 通过一组逻辑操作单元（一组DML——sql语句），将数据从一种状态切换到另外一种状态s
>
> **事务**： 一个或一组sql语句组成一个执行单元，这个执行单元要么全部执行，要么全部不执行
>
> **特点**：
>
> - **原子性**：要么都执行，要么都回滚
> - **一致性**：保证数据的状态操作前和操作后保持一致
> - **隔离性**：多个事务同时操作相同数据库的同一个数据时，一个事务的执行不受另外一个事务的干扰
> - **持久性**：一个事务一旦提交，则数据将持久化到本地，除非其他事务对其进行修改

### 1. 事务的创建

- **隐式事务**： 事务没有明显的开启和结束的标记

  > 比如insert、update、delete语句 

- **显式事务**：事务具有明显的开启和结束的标记

  >  前提：必须先设置自动提交功能为禁用 ==> `set autocommit=0;` 

  - 步骤1：**开启事务**

    ```mysql
    set autocommit=0;
    start transaction;(可选的)
    ```

  - 步骤2：**编写事务中的sql语句**(select insert update delete)

  - 步骤3：**结束事务**： **commit;提交事务、rollback;回滚事务**

- 使用到的关键字

  ```mysql
  set autocommit=0;
  start transaction;
  commit;
  rollback;
  
  savepoint  断点 # 设置保存点
  commit to 断点
  rollback to 断点
  ```

### 2. 事务的隔离级别

> 事务**并发问题**如何发生： 当多个事务同时操作同一个数据库的相同数据时
>
> - **脏读**：一个事务读取到了另外一个事务未提交的数据
> - **不可重复读**：同一个事务中，多次读取到的数据不一致
> - **幻读**：一个事务读取数据时，另外一个事务进行更新，导致第一个事务读取到了没有更新的数据
>
> 避免事务的并发问题：
>
> - 通过设置事务的隔离级别： `set session|global  transaction isolation level 隔离级别名;`
>
>   > 查看隔离级别：`select @@tx_isolation;` 
>
>   - `READ UNCOMMITTED` 
>   - `READ COMMITTED`  可以避免脏读
>   - `REPEATABLE READ`  可以避免脏读、不可重复读和一部分幻读
>   - `SERIALIZABLE` 可以避免脏读、不可重复读和幻读

|                  | 脏读 | 不可重复读 | 幻读 |
| :--------------: | :--: | :--------: | :--: |
| read uncommitted |  √   |     √      |  √   |
|  read committed  |  ×   |     √      |  √   |
| repeatable read  |  ×   |     ×      |  √   |
|   serializable   |  ×   |     ×      |  ×   |

> mysql中默认第三个隔离级别 repeatable read
> oracle中默认第二个隔离级别 read committed

- **演示事务的使用步骤**

  - 开启事务

    ```mysql
    SET autocommit=0;
    START TRANSACTION;
    ```

  - 编写一组事务的语句 

    ```mysql
    UPDATE account SET balance = 1000 WHERE username='张无忌';
    UPDATE account SET balance = 1000 WHERE username='赵敏';
    ```

  - 结束事务

    ```mysql
    ROLLBACK;
    ```

- **演示savepoint 的使用**

  ```mysql
  SET autocommit=0;
  START TRANSACTION;
  
  DELETE FROM account WHERE id=25;
  #设置保存点
  SAVEPOINT a;
  
  DELETE FROM account WHERE id=28;
  #回滚到保存点
  ROLLBACK TO a;
  ```

## 2. 视图

>  **含义**： 理解成一张虚拟的表，和普通表一样使用

**视图和表的区别：**

|      | 使用方式 |        占用物理空间         |
| :--: | :------: | :-------------------------: |
| 视图 | 完全相同 | 不占用，仅仅保存的是sql逻辑 |
|  表  | 完全相同 |            占用             |

**视图的好处：**

- sql语句提高重用性，效率高
- 和表实现了分离，提高了安全性

### 1. 创建视图

```mysql
CREATE VIEW  视图名
AS
查询语句;
```

- 案例1： 查询姓名中包含a字符的员工名、部门名和工种信息

  - 创建

    ```mysql
    CREATE VIEW myv1
    AS
    SELECT last_name,department_name,job_title
    FROM employees e
    JOIN departments d ON e.department_id  = d.department_id
    JOIN jobs j ON j.job_id  = e.job_id;
    ```

  - 使用

    ```mysql
    SELECT * FROM myv1 WHERE last_name LIKE '%a%';
    ```

- 案例2： 查询各部门的平均工资级别

  - 创建视图查看每个部门的平均工资

    ```mysql
    CREATE VIEW myv2
    AS
    SELECT AVG(salary) ag,department_id
    FROM employees
    GROUP BY department_id;
    ```

  - 使用

    ```mysql
    SELECT myv2.ag,g.grade_level
    FROM myv2
    JOIN job_grades g
    ON myv2.ag BETWEEN g.lowest_sal AND g.highest_sal;
    ```

- 案例3： 查询平均工资最低的部门名和工资

  ```mysql
  # 创建
  CREATE VIEW myv3
  AS
  SELECT * FROM myv2 ORDER BY ag LIMIT 1;
  # 使用
  SELECT d.*,m.ag
  FROM myv3 m
  JOIN departments d
  ON m.department_id=d.department_id;
  ```

### 2. 视图的修改

```mysql
# 方式一：
create or replace view  视图名
as
查询语句;

# 方式二：
语法：
alter view 视图名
as 
查询语句;
```

例子：

```mysql
# 方式一：
CREATE OR REPLACE VIEW myv3
AS
SELECT AVG(salary),job_id
FROM employees
GROUP BY job_id;

# 方式二：
ALTER VIEW myv3
AS
SELECT * FROM employees;
```

### 3. 删除视图

```mysql
drop view 视图名,视图名,...;
```

### 4. 查看视图

```mysql
DESC myv3;
SHOW CREATE VIEW myv3;
```

### 5. 视图的更新

> 某些视图不能更新： 
>
> - 包含以下关键字的sql语句：分组函数、distinct、group  by、having、union或者union all
> - 常量视图
> - Select中包含子查询
> - join
> - from 一个不能更新的视图
> - where 子句的子查询引用了from子句中的表

## 3. 存储过程

> 含义：一组经过预先编译的sql语句的集合(理解成批处理语句)
> 好处：
>
> - 提高了sql语句的重用性，减少了开发程序员的压力
> - 提高了效率
> - 减少了传输次数

### 1. 创建存储过程

```mysql
create procedure 存储过程名(in|out|inout 参数名  参数类型,...)
begin
	存储过程体（一组合法的SQL语句）
end
```

> **注意**：
>
> - 参数列表包含三部分：**参数模式  参数名  参数类型**
>
>   **参数模式：**
>
>   - `in`：该参数可以作为输入，也就是该参数需要调用方传入值
>   - `out`：该参数可以作为输出，也就是该参数可以作为返回值
>   - `inout`：该参数既可以作为输入又可以作为输出，也就是该参数既需要传入值，又可以返回值
>
>   举例：`in stuname varchar(20)`
>
> - 存储过程体中可以有多条sql语句，如果仅仅一条sql语句，则可以省略begin end
>
>   - 存储过程体中的每条sql语句的结尾要求必须加分号
>   - 存储过程的结尾可以使用 delimiter 重新设置
>
>   > 语法：`delimiter 结束标记` 
>   > 案例：delimiter $

### 2. 调用存储过程

```mysql
call 存储过程名(实参列表)
```

- **空参列表**

  - 案例：插入到admin表中五条记录

    ```mysql
    DELIMITER $
    CREATE PROCEDURE myp1()
    BEGIN
    	INSERT INTO admin(username,`password`) 
    	VALUES('john1','0000'),('lily','0000'),('rose','0000'),('jack','0000'),('tom','0000');
    END $
    
    # 调用
    CALL myp1()$
    ```

- 创建带 in 模式参数的存储过程
  - 案例1：创建存储过程实现 根据女神名，查询对应的男神信息

    ```mysql
    CREATE PROCEDURE myp2(IN beautyName VARCHAR(20))
    BEGIN
    	SELECT bo.*
    	FROM boys bo
    	RIGHT JOIN beauty b ON bo.id = b.boyfriend_id
    	WHERE b.name = beautyName;
    END $
    
    # 调用
    CALL myp2('柳岩')$
    ```

  - 案例2 ：创建存储过程实现，用户是否登录成功

    ```mysql
    CREATE PROCEDURE myp4(IN username VARCHAR(20),IN PASSWORD VARCHAR(20))
    BEGIN
    	DECLARE result INT DEFAULT 0;#声明并初始化
    	SELECT COUNT(*) INTO result#赋值
    	FROM admin
    	WHERE admin.username = username
    	AND admin.password = PASSWORD;
    
    	SELECT IF(result>0,'成功','失败');#使用
    END $
    
    # 调用
    CALL myp3('张飞','8888')$
    ```

- 创建 out 模式参数的存储过程
  - 案例1：根据输入的女神名，返回对应的男神名

    ```mysql
    CREATE PROCEDURE myp6(IN beautyName VARCHAR(20),OUT boyName VARCHAR(20))
    BEGIN
    	SELECT bo.boyname INTO boyname
    	FROM boys bo
    	RIGHT JOIN
    	beauty b ON b.boyfriend_id = bo.id
    	WHERE b.name=beautyName ;
    END $
    ```

  - 案例2：根据输入的女神名，返回对应的男神名和魅力值

    ```mysql
    CREATE PROCEDURE myp7(IN beautyName VARCHAR(20),OUT boyName VARCHAR(20),OUT usercp INT) 
    BEGIN
    	SELECT boys.boyname ,boys.usercp INTO boyname,usercp
    	FROM boys 
    	RIGHT JOIN
    	beauty b ON b.boyfriend_id = boys.id
    	WHERE b.name=beautyName ;	
    END $
    
    # 调用
    # 系统为了区分系统变量，规定用户自定义变量必须使用一个 @ 符号
    CALL myp7('小昭',@name,@cp)$
    SELECT @name,@cp$
    ```

- 创建带 inout 模式参数的存储过程
  - 案例1：传入a和b两个值，最终a和b都翻倍并返回

    ```mysql
    CREATE PROCEDURE myp8(INOUT a INT ,INOUT b INT)
    BEGIN
    	SET a=a2;
    	SET b=b2;
    END $
    
    # 调用
    SET @m=10
    SET @n=20
    CALL myp8(@m,@n)
    SELECT @m,@n
    ```

### 3. 删除存储过程

```mysql
drop procedure 存储过程名(只能有一个过程名)
```

### 4. 查看存储过程的信息

```mysql
SHOW CREATE PROCEDURE  myp2;
```

## 4. 函数

> **含义**：一组预先编译好的SQL语句的集合(理解成批处理语句)
>
> **优点**：
>
> - 提高代码的重用性
> - 简化操作
> - 减少了编译次数并且减少了和数据库服务器的连接次数，提高了效率
>
> **区别**：
>
> - **存储过程**：可以有 0 个返回，也可以有多个返回，**适合做批量插入、批量更新**
> - **函数**：有且仅有 1 个返回，**适合做处理数据后返回一个结果**

|          |  关键字   |    调用语法     |     返回值      |           应用场景           |
| :------: | :-------: | :-------------: | :-------------: | :--------------------------: |
|   函数   | FUNCTION  |  SELECT 函数()  |   只能是一个    | 用于查询结果为一个值并返回时 |
| 存储过程 | PROCEDURE | CALL 存储过程() | 可以有0个或多个 |           用于更新           |

### 1. 创建函数

> **注意：**
>
> - 参数列表包含两部分： 参数名 参数类型
>
> - 函数体：肯定会有return语句，如果没有会报错；如果return语句没有放在函数体的最后也不报错，但不建议 return 值;
>
> - 函数体中仅有一句话，则可以省略begin end
> - 使用 delimiter 语句设置结束标记

```mysql
CREATE FUNCTION 函数名(参数名 参数类型,...) RETURNS 返回类型
BEGIN
	函数体
END
```

### 2. 调用函数

```mysql
SELECT 函数名（实参列表）
```

**案例演示**： 

- **无参有返回**

  - 案例：返回公司的员工个数

    ```mysql
    CREATE FUNCTION myf1() RETURNS INT
    BEGIN
    	DECLARE c INT DEFAULT 0;#定义局部变量
    	SELECT COUNT(*) INTO c#赋值
    	FROM employees;
    	
    	RETURN c;
    END $
    
    # 调用
    SELECT myf1()$
    ```

- **有参有返回**
  - 案例1：根据员工名，返回它的工资

    ```mysql
    CREATE FUNCTION myf2(empName VARCHAR(20)) RETURNS DOUBLE
    BEGIN
    	SET @sal=0;#定义用户变量 
    	SELECT salary INTO @sal   #赋值
    	FROM employees
    	WHERE last_name = empName;
    	
    	RETURN @sal;
    END $
    
    # 调用
    SELECT myf2('k_ing') $
    ```

  - 案例2：根据部门名，返回该部门的平均工资

    ```mysql
    CREATE FUNCTION myf3(deptName VARCHAR(20)) RETURNS DOUBLE
    BEGIN
    	DECLARE sal DOUBLE ;
    	SELECT AVG(salary) INTO sal
    	FROM employees e
    	JOIN departments d ON e.department_id = d.department_id
    	WHERE d.department_name=deptName;
    	
    	RETURN sal;
    END $
    
    # 调用
    SELECT myf3('IT')$
    ```

### 3. 查看函数

```mysql
SHOW CREATE FUNCTION myf3;
```

### 4. 删除函数

```mysql
DROP FUNCTION myf3;
```

- 案例：创建函数，实现传入两个float，返回二者之和

  ```mysql
  CREATE FUNCTION test_fun1(num1 FLOAT,num2 FLOAT) RETURNS FLOAT
  BEGIN
  	DECLARE res FLOAT DEFAULT 0;
  	SET res = num1+num2;
  	
  	RETURN res;
  END $
  
  # 调用
  SELECT test_fun1(1,2)$
  ```

## 5. 变量

### 1. 系统变量

> **说明**：变量由系统定义，不是用户定义，属于服务器层面
> **注意**：
>
> - 全局变量需要添加 `global` 关键字
> - 会话变量需要添加 `session` 关键字
> - 如果不写，默认会话级别
>
> **使用步骤**：
>
> - 查看所有系统变量：` show global|【session】variables;` 
> - 查看满足条件的部分系统变量： `show global|【session】 variables like '%char%';` 
> - 查看指定的系统变量的值：`select @@global|【session】系统变量名;` 
> - 为某个系统变量赋值
>   - 方式一： `set global|【session】系统变量名=值;`
>   - 方式二： `set @@global|【session】系统变量名=值;` 

#### 1. 全局变量

> **作用域**：针对于所有会话（连接）有效，但不能跨重启

- 查看所有全局变量： `SHOW GLOBAL VARIABLES;`
- 查看满足条件的部分系统变量： `SHOW GLOBAL VARIABLES LIKE '%char%';`
- 查看指定的系统变量的值： `SELECT @@global.autocommit;`
- 为某个系统变量赋值： `SET @@global.autocommit=0;` 或 `SET GLOBAL autocommit=0;`

#### 2. 会话变量

> **作用域**：针对于当前会话（连接）有效

- 查看所有会话变量： `SHOW SESSION VARIABLES;`
- 查看满足条件的部分会话变量： `SHOW SESSION VARIABLES LIKE '%char%';`
- 查看指定的会话变量的值： `SELECT @@autocommit;` 或 `SELECT @@session.tx_isolation;`
- 为某个会话变量赋值： `SET @@session.tx_isolation='read-uncommitted';` 或 `SET SESSION tx_isolation='read-committed';`

### 2. 自定义变量

> **说明**：变量由用户自定义，而不是系统提供的
> **使用步骤**： 声明、赋值、使用（查看、比较、运算等）

#### 1. 用户变量

> **作用域**：针对于当前会话（连接）有效，作用域同于会话变量
>
> **赋值操作符**：`= 或 :=` 

- **声明并初始化**

  ```mysql
  SET @变量名=值;
  SET @变量名:=值;
  SELECT @变量名:=值;
  ```

- **赋值**（更新变量的值）

  - 方式一：一般用于赋简单的值

    ```mysql
    SET @变量名=值;
    SET @变量名:=值;
    SELECT @变量名:=值;
    ```

  - 方式二：一般用于赋表 中的字段值

    ```mysql
    SELECT 字段 INTO @变量名
    FROM 表;
    ```

- **使用**（查看变量的值）

  ```mysql
  SELECT @变量名;
  ```

#### 2. 局部变量

> **作用域**：仅仅在定义它的begin end块中有效(**BEGIN END的第一句话**)

- **声明**

  ```mysql
  DECLARE 变量名 类型;
  DECLARE 变量名 类型 【DEFAULT 值】;
  ```

- **赋值** 

  - 方式一：一般用于赋简单的值

    ```mysql
    SET 变量名=值;
    SET 变量名:=值;
    SELECT 变量名:=值;
    ```

  - 方式二：一般用于赋表 中的字段值

    ```mysql
    SELECT 字段名或表达式 INTO 变量
    FROM 表;
    ```

- **使用**

  ```mysql
  select 变量名
  ```

- **案例**：声明两个变量，求和并打印
  - 用户变量

    ```mysql
    SET @m=1;
    SET @n=1;
    SET @sum=@m+@n;
    SELECT @sum;
    ```

  - 局部变量

    ```mysql
    DECLARE m INT DEFAULT 1;
    DECLARE n INT DEFAULT 1;
    DECLARE SUM INT;
    SET SUM=m+n;
    SELECT SUM;
    ```

#### 3. 二者的区别：

|          |       作用域        |      定义位置       |           语法           |
| :------: | :-----------------: | :-----------------: | :----------------------: |
| 用户变量 |      当前会话       |   会话的任何地方    |  加@符号，不用指定类型   |
| 局部变量 | 定义它的BEGIN END中 | BEGIN END的第一句话 | 一般不用加@,需要指定类型 |

## 6. 流程控制结构

### 1. 分支

#### 1. if函数

> 特点：可以用在任何位置

```mysql
if(条件，值1，值2)
```

#### 2. case 语句

```mysql
# 情况一：类似于switch
case 表达式
when 值1 then 结果1或语句1(如果是语句，需要加分号) 
when 值2 then 结果2或语句2(如果是语句，需要加分号)
...
else 结果n或语句n(如果是语句，需要加分号)
end 【case】（如果是放在begin end中需要加上case，如果放在select后面不需要）

# 情况二：类似于多重if
case 
when 条件1 then 结果1或语句1(如果是语句，需要加分号) 
when 条件2 then 结果2或语句2(如果是语句，需要加分号)
...
else 结果n或语句n(如果是语句，需要加分号)
end 【case】（如果是放在begin end中需要加上case，如果放在select后面不需要）
```

- 案例1：创建函数，实现传入成绩，如果成绩>90,返回A，如果成绩>80,返回B，如果成绩>60,返回C，否则返回D

  ```mysql
  CREATE FUNCTION test_case(score FLOAT) RETURNS CHAR
  BEGIN 
  	DECLARE ch CHAR DEFAULT 'A';
  	CASE 
  	WHEN score>90 THEN SET ch='A';
  	WHEN score>80 THEN SET ch='B';
  	WHEN score>60 THEN SET ch='C';
  	ELSE SET ch='D';
  	END CASE;
  	RETURN ch;
  END $
  
  # 调用
  SELECT test_case(56)$
  ```

#### 3. if elseif语句

> **特点**： 只能用在begin end中

```mysql
if 情况1 then 语句1;
elseif 情况2 then 语句2;
...
else 语句n;
end if;
```

- 案例1：创建函数，实现传入成绩，如果成绩>90,返回A，如果成绩>80,返回B，如果成绩>60,返回C，否则返回D

  ```mysql
  CREATE FUNCTION test_if(score FLOAT) RETURNS CHAR
  BEGIN
  	DECLARE ch CHAR DEFAULT 'A';
  	IF score>90 THEN SET ch='A';
  	ELSEIF score>80 THEN SET ch='B';
  	ELSEIF score>60 THEN SET ch='C';
  	ELSE SET ch='D';
  	END IF;
  	RETURN ch;	
  END $
  
  # 调用
  SELECT test_if(87)$
  ```

- 案例2：创建存储过程，如果工资<2000,则删除，如果5000>工资>2000,则涨工资1000，否则涨工资500

  ```mysql
  CREATE PROCEDURE test_if_pro(IN sal DOUBLE)
  BEGIN
  	IF sal<2000 THEN DELETE FROM employees WHERE employees.salary=sal;
  	ELSEIF sal>=2000 AND sal<5000 
  		THEN UPDATE employees SET salary=salary+1000 WHERE employees.salary=sal;
  	ELSE UPDATE employees SET salary=salary+500 WHERE employees.salary=sal;
  	END IF;	
  END $
  
  # 调用
  CALL test_if_pro(2100)$
  ```

#### 4. 三者比较：

|           |     应用场合     |
| :-------: | :--------------: |
|  if 函数  |    简单双分支    |
| case 结构 | 等值判断的多分支 |
|  if 结构  | 区间判断的多分支 |

### 2. 循环

> 循环控制：
>
> - `iterate` 类似于 continue，继续，结束本次循环，继续下一次
> - `leave` 类似于  break，跳出，结束当前所在的循环

#### 1. while

```mysql
【标签:】while 循环条件 do
	循环体;
end while【 标签】;
```

#### 2. loop

```mysql
【标签:】loop
	循环体;
end loop 【标签】;
```

#### 3. repeat

```mysql
【标签：】repeat
	循环体;
until 结束循环的条件
end repeat 【标签】;
```

#### 4. 案例

- **没有添加循环控制语句**

  - 案例：批量插入，根据次数插入到admin表中多条记录

    ```mysql
    CREATE PROCEDURE pro_while1(IN insertCount INT)
    BEGIN
    	DECLARE i INT DEFAULT 1;
    	WHILE i<=insertCount DO
    		INSERT INTO admin(username,`password`) VALUES(CONCAT('Rose',i),'666');
    		SET i=i+1;
    	END WHILE;
    END $ 
    
    # 调用
    CALL pro_while1(100)$
    ```

- **添加 leave 语句**
  - 案例：批量插入，根据次数插入到admin表中多条记录，如果次数>20则停止

    ```mysql
    CREATE PROCEDURE test_while1(IN insertCount INT)
    BEGIN
    	DECLARE i INT DEFAULT 1;
    	a:WHILE i<=insertCount DO
    		INSERT INTO admin(username,`password`) VALUES(CONCAT('xiaohua',i),'0000');
    		IF i>=20 THEN LEAVE a;
    		END IF;
    		SET i=i+1;
    	END WHILE a;
    END $
    
    # 调用
    CALL test_while1(100)$
    ```

- **添加 iterate 语句**

  - 案例：批量插入，根据次数插入到admin表中多条记录，只插入偶数次

    ```mysql
    CREATE PROCEDURE test_while1(IN insertCount INT)
    BEGIN
    	DECLARE i INT DEFAULT 0;
    	a:WHILE i<=insertCount DO
    		SET i=i+1;
    		IF MOD(i,2)!=0 THEN ITERATE a;
    		END IF;
    		INSERT INTO admin(username,`password`) VALUES(CONCAT('xiaohua',i),'0000');
    	END WHILE a;
    END $
    
    # 调用
    CALL test_while1(100)$
    ```

---

# 进阶

# 一、索引

## B+ Tree 原理

### 1. 数据结构

B Tree 指的是 Balance Tree，也就是平衡树。平衡树是一颗查找树，并且所有叶子节点位于同一层。

B+ Tree 是基于 B Tree 和叶子节点顺序访问指针进行实现，它具有 B Tree 的平衡性，并且通过顺序访问指针来提高区间查询的性能。

在 B+ Tree 中，一个节点中的 key 从左到右非递减排列，如果某个指针的左右相邻 key 分别是 key<sub>i</sub> 和 key<sub>i+1</sub>，且不为 null，则该指针指向节点的所有 key 大于等于 key<sub>i</sub> 且小于等于 key<sub>i+1</sub>。

<div align="center"> <img src="../pics//061c88c1-572f-424f-b580-9cbce903a3fe.png"/> </div><br>

### 2. 操作

进行查找操作时，首先在根节点进行二分查找，找到一个 key 所在的指针，然后递归地在指针所指向的节点进行查找。直到查找到叶子节点，然后在叶子节点上进行二分查找，找出 key 所对应的 data。

插入删除操作会破坏平衡树的平衡性，因此在插入删除操作之后，需要对树进行一个分裂、合并、旋转等操作来维护平衡性。

### 3. 与红黑树的比较

红黑树等平衡树也可以用来实现索引，但是文件系统及数据库系统普遍采用 B+ Tree 作为索引结构，主要有以下两个原因：

（一）更少的查找次数

平衡树查找操作的时间复杂度等于树高 h，而树高大致为 O(h)=O(log<sub>d</sub>N)，其中 d 为每个节点的出度。

红黑树的出度为 2，而 B+ Tree 的出度一般都非常大，所以红黑树的树高 h 很明显比 B+ Tree 大非常多，查找的次数也就更多。

（二）利用磁盘预读特性

为了减少磁盘 I/O，磁盘往往不是严格按需读取，而是每次都会预读。预读过程中，磁盘进行顺序读取，顺序读取不需要进行磁盘寻道，并且只需要很短的旋转时间，速度会非常快。

操作系统一般将内存和磁盘分割成固态大小的块，每一块称为一页，内存与磁盘以页为单位交换数据。数据库系统将索引的一个节点的大小设置为页的大小，使得一次 I/O 就能完全载入一个节点。并且可以利用预读特性，相邻的节点也能够被预先载入。

## MySQL 索引

索引是在存储引擎层实现的，而不是在服务器层实现的，所以不同存储引擎具有不同的索引类型和实现。

### 1. B+Tree 索引

是大多数 MySQL 存储引擎的默认索引类型。

因为不再需要进行全表扫描，只需要对树进行搜索即可，所以查找速度快很多。

除了用于查找，还可以用于排序和分组。

可以指定多个列作为索引列，多个索引列共同组成键。

适用于全键值、键值范围和键前缀查找，其中键前缀查找只适用于最左前缀查找。如果不是按照索引列的顺序进行查找，则无法使用索引。

InnoDB 的 B+Tree 索引分为主索引和辅助索引。主索引的叶子节点 data 域记录着完整的数据记录，这种索引方式被称为聚簇索引。因为无法把数据行存放在两个不同的地方，所以一个表只能有一个聚簇索引。

<div align="center"> <img src="../pics//c28c6fbc-2bc1-47d9-9b2e-cf3d4034f877.jpg"/> </div><br>

辅助索引的叶子节点的 data 域记录着主键的值，因此在使用辅助索引进行查找时，需要先查找到主键值，然后再到主索引中进行查找。

<div align="center"> <img src="../pics//7ab8ca28-2a41-4adf-9502-cc0a21e63b51.jpg"/> </div><br>

### 2. 哈希索引

哈希索引能以 O(1) 时间进行查找，但是失去了有序性：

- 无法用于排序与分组；
- 只支持精确查找，无法用于部分查找和范围查找。

InnoDB 存储引擎有一个特殊的功能叫“自适应哈希索引”，当某个索引值被使用的非常频繁时，会在 B+Tree 索引之上再创建一个哈希索引，这样就让 B+Tree 索引具有哈希索引的一些优点，比如快速的哈希查找。

### 3. 全文索引

MyISAM 存储引擎支持全文索引，用于查找文本中的关键词，而不是直接比较是否相等。

查找条件使用 MATCH AGAINST，而不是普通的 WHERE。

全文索引使用倒排索引实现，它记录着关键词到其所在文档的映射。

InnoDB 存储引擎在 MySQL 5.6.4 版本中也开始支持全文索引。

### 4. 空间数据索引

MyISAM 存储引擎支持空间数据索引（R-Tree），可以用于地理数据存储。空间数据索引会从所有维度来索引数据，可以有效地使用任意维度来进行组合查询。

必须使用 GIS 相关的函数来维护数据。

## 索引优化

### 1. 独立的列

在进行查询时，索引列不能是表达式的一部分，也不能是函数的参数，否则无法使用索引。

例如下面的查询不能使用 actor_id 列的索引：

```sql
SELECT actor_id FROM sakila.actor WHERE actor_id + 1 = 5;
```

### 2. 多列索引

在需要使用多个列作为条件进行查询时，使用多列索引比使用多个单列索引性能更好。例如下面的语句中，最好把 actor_id 和 film_id 设置为多列索引。

```sql
SELECT film_id, actor_ id FROM sakila.film_actor
WHERE actor_id = 1 AND film_id = 1;
```

### 3. 索引列的顺序

让选择性最强的索引列放在前面。

索引的选择性是指：不重复的索引值和记录总数的比值。最大值为 1，此时每个记录都有唯一的索引与其对应。选择性越高，查询效率也越高。

例如下面显示的结果中 customer_id 的选择性比 staff_id 更高，因此最好把 customer_id 列放在多列索引的前面。

```sql
SELECT COUNT(DISTINCT staff_id)/COUNT(*) AS staff_id_selectivity,
COUNT(DISTINCT customer_id)/COUNT(*) AS customer_id_selectivity,
COUNT(*)
FROM payment;
```

```html
   staff_id_selectivity: 0.0001
customer_id_selectivity: 0.0373
               COUNT(*): 16049
```

### 4. 前缀索引

对于 BLOB、TEXT 和 VARCHAR 类型的列，必须使用前缀索引，只索引开始的部分字符。

对于前缀长度的选取需要根据索引选择性来确定。

### 5. 覆盖索引

索引包含所有需要查询的字段的值。

具有以下优点：

- 索引通常远小于数据行的大小，只读取索引能大大减少数据访问量。
- 一些存储引擎（例如 MyISAM）在内存中只缓存索引，而数据依赖于操作系统来缓存。因此，只访问索引可以不使用系统调用（通常比较费时）。
- 对于 InnoDB 引擎，若辅助索引能够覆盖查询，则无需访问主索引。

## 索引的优点

- 大大减少了服务器需要扫描的数据行数。

- 帮助服务器避免进行排序和分组，以及避免创建临时表（B+Tree 索引是有序的，可以用于 ORDER BY 和 GROUP BY 操作。临时表主要是在排序和分组过程中创建，因为不需要排序和分组，也就不需要创建临时表）。

- 将随机 I/O 变为顺序 I/O（B+Tree 索引是有序的，会将相邻的数据都存储在一起）。

## 索引的使用条件

- 对于非常小的表、大部分情况下简单的全表扫描比建立索引更高效；

- 对于中到大型的表，索引就非常有效；

- 但是对于特大型的表，建立和维护索引的代价将会随之增长。这种情况下，需要用到一种技术可以直接区分出需要查询的一组数据，而不是一条记录一条记录地匹配，例如可以使用分区技术。

# 二、查询性能优化

## 使用 Explain 进行分析

Explain 用来分析 SELECT 查询语句，开发人员可以通过分析 Explain 结果来优化查询语句。

比较重要的字段有：

- select_type : 查询类型，有简单查询、联合查询、子查询等
- key : 使用的索引
- rows : 扫描的行数

## 优化数据访问

### 1. 减少请求的数据量

- 只返回必要的列：最好不要使用 SELECT * 语句。
- 只返回必要的行：使用 LIMIT 语句来限制返回的数据。
- 缓存重复查询的数据：使用缓存可以避免在数据库中进行查询，特别在要查询的数据经常被重复查询时，缓存带来的查询性能提升将会是非常明显的。

### 2. 减少服务器端扫描的行数

最有效的方式是使用索引来覆盖查询。

## 重构查询方式

### 1. 切分大查询

一个大查询如果一次性执行的话，可能一次锁住很多数据、占满整个事务日志、耗尽系统资源、阻塞很多小的但重要的查询。

```sql
DELEFT FROM messages WHERE create < DATE_SUB(NOW(), INTERVAL 3 MONTH);
```

```sql
rows_affected = 0
do {
    rows_affected = do_query(
    "DELETE FROM messages WHERE create  < DATE_SUB(NOW(), INTERVAL 3 MONTH) LIMIT 10000")
} while rows_affected > 0
```

### 2. 分解大连接查询

将一个大连接查询分解成对每一个表进行一次单表查询，然后在应用程序中进行关联，这样做的好处有：

- 让缓存更高效。对于连接查询，如果其中一个表发生变化，那么整个查询缓存就无法使用。而分解后的多个查询，即使其中一个表发生变化，对其它表的查询缓存依然可以使用。
- 分解成多个单表查询，这些单表查询的缓存结果更可能被其它查询使用到，从而减少冗余记录的查询。
- 减少锁竞争；
- 在应用层进行连接，可以更容易对数据库进行拆分，从而更容易做到高性能和可伸缩。
- 查询本身效率也可能会有所提升。例如下面的例子中，使用 IN() 代替连接查询，可以让 MySQL 按照 ID 顺序进行查询，这可能比随机的连接要更高效。

```sql
SELECT * FROM tab
JOIN tag_post ON tag_post.tag_id=tag.id
JOIN post ON tag_post.post_id=post.id
WHERE tag.tag='mysql';
```

```sql
SELECT * FROM tag WHERE tag='mysql';
SELECT * FROM tag_post WHERE tag_id=1234;
SELECT * FROM post WHERE post.id IN (123,456,567,9098,8904);
```

# 三、存储引擎

## InnoDB

是 MySQL 默认的事务型存储引擎，只有在需要它不支持的特性时，才考虑使用其它存储引擎。

实现了四个标准的隔离级别，默认级别是可重复读（REPEATABLE READ）。在可重复读隔离级别下，通过多版本并发控制（MVCC）+ 间隙锁（Next-Key Locking）防止幻影读。

主索引是聚簇索引，在索引中保存了数据，从而避免直接读取磁盘，因此对查询性能有很大的提升。

内部做了很多优化，包括从磁盘读取数据时采用的可预测性读、能够加快读操作并且自动创建的自适应哈希索引、能够加速插入操作的插入缓冲区等。

支持真正的在线热备份。其它存储引擎不支持在线热备份，要获取一致性视图需要停止对所有表的写入，而在读写混合场景中，停止写入可能也意味着停止读取。

## MyISAM

设计简单，数据以紧密格式存储。对于只读数据，或者表比较小、可以容忍修复操作，则依然可以使用它。

提供了大量的特性，包括压缩表、空间数据索引等。

不支持事务。

不支持行级锁，只能对整张表加锁，读取时会对需要读到的所有表加共享锁，写入时则对表加排它锁。但在表有读取操作的同时，也可以往表中插入新的记录，这被称为并发插入（CONCURRENT INSERT）。

可以手工或者自动执行检查和修复操作，但是和事务恢复以及崩溃恢复不同，可能导致一些数据丢失，而且修复操作是非常慢的。

如果指定了 DELAY_KEY_WRITE 选项，在每次修改执行完成时，不会立即将修改的索引数据写入磁盘，而是会写到内存中的键缓冲区，只有在清理键缓冲区或者关闭表的时候才会将对应的索引块写入磁盘。这种方式可以极大的提升写入性能，但是在数据库或者主机崩溃时会造成索引损坏，需要执行修复操作。

## 比较

- 事务：InnoDB 是事务型的，可以使用 Commit 和 Rollback 语句。

- 并发：MyISAM 只支持表级锁，而 InnoDB 还支持行级锁。

- 外键：InnoDB 支持外键。

- 备份：InnoDB 支持在线热备份。

- 崩溃恢复：MyISAM 崩溃后发生损坏的概率比 InnoDB 高很多，而且恢复的速度也更慢。

- 其它特性：MyISAM 支持压缩表和空间数据索引。

# 四、数据类型

## 整型

TINYINT, SMALLINT, MEDIUMINT, INT, BIGINT 分别使用 8, 16, 24, 32, 64 位存储空间，一般情况下越小的列越好。

INT(11) 中的数字只是规定了交互工具显示字符的个数，对于存储和计算来说是没有意义的。

## 浮点数

FLOAT 和 DOUBLE 为浮点类型，DECIMAL 为高精度小数类型。CPU 原生支持浮点运算，但是不支持 DECIMAl 类型的计算，因此 DECIMAL 的计算比浮点类型需要更高的代价。

FLOAT、DOUBLE 和 DECIMAL 都可以指定列宽，例如 DECIMAL(18, 9) 表示总共 18 位，取 9 位存储小数部分，剩下 9 位存储整数部分。

## 字符串

主要有 CHAR 和 VARCHAR 两种类型，一种是定长的，一种是变长的。

VARCHAR 这种变长类型能够节省空间，因为只需要存储必要的内容。但是在执行 UPDATE 时可能会使行变得比原来长，当超出一个页所能容纳的大小时，就要执行额外的操作。MyISAM 会将行拆成不同的片段存储，而 InnoDB 则需要分裂页来使行放进页内。

在进行存储和检索时，会保留 VARCHAR 末尾的空格，而会删除 CHAR 末尾的空格。

## 时间和日期

MySQL 提供了两种相似的日期时间类型：DATETIME 和 TIMESTAMP。

### 1. DATETIME

能够保存从 1001 年到 9999 年的日期和时间，精度为秒，使用 8 字节的存储空间。

它与时区无关。

默认情况下，MySQL 以一种可排序的、无歧义的格式显示 DATETIME 值，例如“2008-01-16 22:37:08”，这是 ANSI 标准定义的日期和时间表示方法。

### 2. TIMESTAMP

和 UNIX 时间戳相同，保存从 1970 年 1 月 1 日午夜（格林威治时间）以来的秒数，使用 4 个字节，只能表示从 1970 年到 2038 年。

它和时区有关，也就是说一个时间戳在不同的时区所代表的具体时间是不同的。

MySQL 提供了 FROM_UNIXTIME() 函数把 UNIX 时间戳转换为日期，并提供了 UNIX_TIMESTAMP() 函数把日期转换为 UNIX 时间戳。

默认情况下，如果插入时没有指定 TIMESTAMP 列的值，会将这个值设置为当前时间。

应该尽量使用 TIMESTAMP，因为它比 DATETIME 空间效率更高。

# 五、切分

## 水平切分

水平切分又称为 Sharding，它是将同一个表中的记录拆分到多个结构相同的表中。

当一个表的数据不断增多时，Sharding 是必然的选择，它可以将数据分布到集群的不同节点上，从而缓存单个数据库的压力。

<div align="center"> <img src="../pics//63c2909f-0c5f-496f-9fe5-ee9176b31aba.jpg"/> </div><br>

## 垂直切分

垂直切分是将一张表按列切分成多个表，通常是按照列的关系密集程度进行切分，也可以利用垂直切分将经常被使用的列和不经常被使用的列切分到不同的表中。

在数据库的层面使用垂直切分将按数据库中表的密集程度部署到不同的库中，例如将原来的电商数据库垂直切分成商品数据库、用户数据库等。

<div align="center"> <img src="../pics//e130e5b8-b19a-4f1e-b860-223040525cf6.jpg"/> </div><br>

## Sharding 策略

- 哈希取模：hash(key) % N；
- 范围：可以是 ID 范围也可以是时间范围；
- 映射表：使用单独的一个数据库来存储映射关系。

## Sharding 存在的问题

### 1. 事务问题

使用分布式事务来解决，比如 XA 接口。

### 2. 连接

可以将原来的连接分解成多个单表连接查询，然后在用户程序中进行连接。

### 3. ID 唯一性

- 使用全局唯一 ID（GUID）
- 为每个分片指定一个 ID 范围
- 分布式 ID 生成器 (如 Twitter 的 Snowflake 算法)

# 六、复制

## 主从复制

主要涉及三个线程：binlog 线程、I/O 线程和 SQL 线程。

-  **binlog 线程** ：负责将主服务器上的数据更改写入二进制日志（Binary log）中。
-  **I/O 线程** ：负责从主服务器上读取二进制日志，并写入从服务器的重放日志（Replay log）中。
-  **SQL 线程** ：负责读取重放日志并重放其中的 SQL 语句。

<div align="center"> <img src="../pics//master-slave.png"/> </div><br>

## 读写分离

主服务器处理写操作以及实时性要求比较高的读操作，而从服务器处理读操作。

读写分离能提高性能的原因在于：

- 主从服务器负责各自的读和写，极大程度缓解了锁的争用；
- 从服务器可以使用 MyISAM，提升查询性能以及节约系统开销；
- 增加冗余，提高可用性。

读写分离常用代理方式来实现，代理服务器接收应用层传来的读写请求，然后决定转发到哪个服务器。

 <img src="../pics//master-slave-proxy.png"/>

# 参考资料

- BaronScbwartz, PeterZaitsev, VadimTkacbenko, 等. 高性能 MySQL[M]. 电子工业出版社, 2013.
- 姜承尧. MySQL 技术内幕: InnoDB 存储引擎 [M]. 机械工业出版社, 2011.
- [20+ 条 MySQL 性能优化的最佳经验](https://www.jfox.info/20-tiao-mysql-xing-nen-you-hua-de-zui-jia-jing-yan.html)
- [服务端指南 数据存储篇 | MySQL（09） 分库与分表带来的分布式困境与应对之策](http://blog.720ui.com/2017/mysql_core_09_multi_db_table2/ "服务端指南 数据存储篇 | MySQL（09） 分库与分表带来的分布式困境与应对之策")
- [How to create unique row ID in sharded databases?](https://stackoverflow.com/questions/788829/how-to-create-unique-row-id-in-sharded-databases)
- [SQL Azure Federation – Introduction](http://geekswithblogs.net/shaunxu/archive/2012/01/07/sql-azure-federation-ndash-introduction.aspx "Title of this entry.")
- [MySQL 索引背后的数据结构及算法原理](http://blog.codinglabs.org/articles/theory-of-mysql-index.html)
- [MySQL 性能优化神器 Explain 使用分析](https://segmentfault.com/a/1190000008131735)
- [How Sharding Works](https://medium.com/@jeeyoungk/how-sharding-works-b4dec46b3f6)
- [大众点评订单系统分库分表实践](https://tech.meituan.com/dianping_order_db_sharding.html)