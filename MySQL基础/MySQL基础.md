# 1. 数据库相关概念

1、DB：全称数据库，用于存放有组织的数据

2、DBMS：数据库管理系统，又称之为数据库软件（产品），用于管理DB中的数据

3、SQL：结构化查询语言，用于和DBMS进行通信的语言。



# 2.数据库存储数据的特点

1. 将数据放入表中，再将表放入库中。
2. 一个数据库可以有多个表，每个表都有一个表名，且表名唯一。
3. 表具有一些特性，这些特性定义了数据在表中如何存储，类似于java中“类”的设计
4. 表由列组成，在数据库中列称为字段。所有的表都是由一个列或多个列组成的。
5. 表中的数据按照行来存储 。

# 3. MYSQL产品的介绍与安装

## 3.1 MySQL服务的启动与停止

方式一：计算机-右键管理-服务和应用程序-服务

**方式二： 通过windows命令行启动与停止**

net start MySQL服务名（启动mysql服务）

net stop MySQL服务名（停止mysql服务）

## 3.2 MySQL服务的登录与退出

方式一： 使用mysql自带的客户端进行登录

​		缺点：只限于root用户 ，不够灵活

方式二： 使用windows自带的cmd窗口

​		mysql 【-h主机名 -P端口号】 -u用户名 -p密码

​		**注意**：   -p密码 之间**不能有空格**

退出： exit 或者ctrl+c



## 3.3 MySQL常见命令介绍

```
1.查看当前所有的数据库

	show databases;

2.打开指定的库

	use 数据库名;

3.查看当前库的所有表

	show tables ;

4.查看其他库的所有表

	show tables from 库名；

5.创建表

	create table 表名（

		列名1，列类型，

		列名2，列类型

	）;

6.查看表结构

	desc 表名;

7.查看服务器的版本

方式一：登录到mysql服务端，select version();

方式二：没有登录到mysql服务端

	mysql --version

	mysql -V

```

## 3.4 MySQL语法规范介绍

```
1.不区分大小写。
2.结尾建议使用";" 也可以使用\g
3.每条命令根据需要，进行缩进或换行
4.注释
	单行注释 #注释文字
	单行注释 -- 注释文字
	多行注释 /* 注释文字 */
```

## 3.5 SQL语言分类

```
DQL(Data Query Language):数据库查询语言
	select
DML(Data Manipulate Language):数据库操作语言
	update delete insert 
DDL(Data Define Language):数据定义语言,数据库表的操作
	create、drop、alter
TCL(Trasaction Control Language):事务控制语言
	commit、rollback
```



# 4 .DQL语言的学习

## 进阶1：基础查询

```
语法：
SELECT 要查询的东西
【FROM 表名】;

类似于Java中 :System.out.println(要打印的东西);
特点：
①通过select查询完的结果 ，是一个虚拟的表格，不是真实存在
② 要查询的东西 可以是常量值、可以是表达式、可以是字段、可以是函数
```

## 进阶2：条件查询

```
语法：
   select 
		查询列表
   from
		表名
   where
		筛选条件;
	
分类：
    一、按条件表达式筛选
		简单条件运算符：< > = != <= >= <> 
    二、按逻辑表达式筛选
    	逻辑运算符的作用：用于连接条件表达式
		逻辑运算符：&& ||  !
				  and or not
    三、模糊查询
		like
		between and 
		in 
		is null 
```

```
案例：查询出员工名中第二个字符为_的员工名
分析：
	这时需要通过转义
	方式一：通过"\"进行转义
	select 
			last_name 
	from 	
			employees
	where 
			last_name like '_\_%';
	方式二：通过ESCAPE关键词进行转义
	select
			last_name
	from
			employess
	where
			last_name like '_$_%' ESCAPE '$';
```

## 进阶3：排序查询

```
语法：
	select
		查询列表
	from
		表名
	where	
		条件
	order by 排序的字段|表达式|函数名 [asc | desc];
特点：
	1.asc是升序，desc是降序
	2.默认是asc
	3.order by子句中可以支持单个字段，多个字段，表达式、函数、别名
	4.order by子句一般放在查询语句的最后面，limit子句除外。
```

相关案例：

```sql
#案例2：查询部门编号大于等于90的员工信息，按入职时间的先后进行排序
SELECT
	*
FROM
	employees
WHERE
	department_id >= 90
ORDER BY
	hiredate ;

#案例3：按年薪的高低显示员工的信息和年薪【按表达式排序】
SELECT
	last_name,
	salary * 12 * (1+IFNULL(commission_pct,0)) 年薪
FROM
	employees
ORDER BY salary * 12 * (1+IFNULL(commission_pct,0));


#案例4：按年薪的高低显示员工的信息和年薪【按别名排序】
SELECT
	last_name,
	salary * 12 * (1+IFNULL(commission_pct,0)) 年薪
FROM
	employees
ORDER BY 年薪 ;


#案例5：按姓名的长度显示员工的姓名和工资【按函数排序】
SELECT
	last_name,
	salary
FROM
	employees
ORDER BY LENGTH(last_name) DESC;


#案例6：查询员工信息，要求先按工资排序，再按员工编号排序
SELECT 
	*
FROM
	employees
ORDER BY salary DESC ,employee_id DESC;
```

## 进阶4：常见函数

函数的概念：类似于java中的方法，将一组逻辑语句封装到方法中，对外暴露方法名

函数的好处：

​			1.隐藏了实现细节

​			2.提高代码的重用性

函数的调用：select 函数名() 【from 表名】

函数的特点：

​			①叫什么（函数名）

​			②干什么（函数功能）

分类：

​		1.单行函数

​		如：concat(),length(),ifnull()

​		2.**分组函数**：做统计使用，因此又称为统计函数、**聚合函数**、组函数



### 一、单行函数

```
1.字符函数
    length:获取字节数的个数
    concat:拼接函数
    upper:转换成大写
    lower:转换成小写
    replace:替换
    lpad:左填充，在字符左边填充指定字符，指定填充完之后字符的总长度
    rpad:右填充，在字符右边填充指定字符，指定填充完之后字符的总长度
    trim:去前后指定的空格和字符
    ltrim:去左边空格
    rtrim:去右边空格
    instr:返回子串在字符串中第一次出现的索引
    substr:截取字符
```

相关示例：

```sql
#一、单行函数
#length:获取字节数的个数
SELECT LENGTH('john');
SELECT LENGTH('张无忌hahaha') out_put; #utf8字符集 一个中文=3个字节

#concat:拼接函数
SELECT CONCAT(first_name,'_',last_name) out_put FROM employees ;

#upper
SELECT UPPER('guaguga');

#  lower:转换成小写
SELECT LOWER('dDSAKJ') out_put;

#lpad:左填充，在字符左边填充指定字符，指定填充完之后字符的总长度
SELECT LPAD('张三丰',9,'a') out_put;

#rpad:右填充，在字符右边填充指定字符，指定填充完之后字符的总长度
SELECT RPAD('张三丰',1,'$') out_put;

#trim:去前后指定的空格和字符
SELECT LENGTH(TRIM('               张三丰                ')) out_put;
SELECT LENGTH('               张三丰                ');

#trim:去前后指定字符a
SELECT TRIM('a' FROM 'aaaaaaaaaaaaaaa张aa三aa丰aaaaaaa') out_put ;

#ltrim:去左边空格
SELECT LTRIM('               张三丰   ' ) out_put;   

#rtrim:去右边空格
SELECT RTRIM('               张三丰   ')  out_put;

#instr:返回子串在字符串中第一次出现的索引
SELECT INSTR('张赵敏无忌爱上了赵敏','赵敏') out_put;

#substr:截取字符,从指定索引截取之后的所有字符
SELECT SUBSTR('张赵敏无忌爱上了赵敏',6) out_put;

#substr:截取字符,从指定索引截取指定长度
SELECT SUBSTR('张赵敏无忌爱上了赵敏',4,7) out_put;
```



```
2.数学函数
	round:四舍五入
	ceil:向上取整，大于等于该参数的最小整数
	floor:向下取整，小于等于该参数的最大整数
	mod:取余   
	truncate：截断
```

相关示例：

```
#round:四舍五入
SELECT ROUND(2.2);
SELECT ROUND(2.78,1); #第二个参数：保留几位小数

#ceil:向上取整，大于等于该参数的最小整数
SELECT CEIL(2.78);
SELECT CEIL(2.0);

#floor:向下取整，小于等于该参数的最大整数
SELECT FLOOR(4.98);

#mod:取余
SELECT MOD(-10,3);

#truncate：截断
SELECT TRUNCATE(7.88,1);#第二个参数:保留几位小数
```



```
3.日期函数
	now:当前系统日期和时间
	curDate:当前系统日期
	curTime:当前系统时间
	str_to_date:将字符转换成日期
	date_format:将日期转换成字符串
```



```
4.流程控制函数
	if 处理双分支
        案例：
        SELECT IF(5>2,'大','小');
        SELECT last_name,commission_pct,IF(commission_pct IS NULL,'没奖金','有奖金') 备注
        FROM employees;	
    case语句 处理多分支
    	情况1：处理等值判断
            case 要判断条件的字段或表达式
            when 常量1 then 要显示的值1或语句1；
            when 常量1 then 要显示的值1或语句1；
            ....
            else 要显示的值n或语句n;
            end
    	情况2：处理条件判断
    		case 
    		when 条件1 then 要显示的值1或语句1；
    		when 条件2 then 要显示的值1或语句1；
    		...
    		else 要显示的值n或语句n;
    		end
```

流程控制函数相关示例：

```sql
/*
   查询员工的工资，要求
	department_id = 30,工资为原来的1.1倍
	department_id = 40,工资为原来的1.2倍
	department_id = 50,工资为原来的1.3倍
	其他情况，原来的工资
*/
    SELECT 
          last_name,
          department_id,
          CASE
            department_id 
            WHEN department_id = 30 
            THEN salary * 1.1 
            WHEN department_id = 40 
            THEN salary * 1.2 
            WHEN department_id = 50 
            THEN salary * 1.3 
            ELSE salary 
            END AS 新工资 
    FROM
      	employees ;


/*
   查询员工的工资情况，要求
	salary >20000,A等级
	salary >15000,A等级
	salary >10000,A等级
	其他情况，D等级
*/
SELECT last_name,salary,
    CASE
    WHEN salary > 20000 THEN 'A'
    WHEN salary > 15000 THEN 'B'
    WHEN salary > 10000 THEN 'C'
    ELSE 'D'
    END AS 工资等级
FROM employees;
```



```
5.其他函数
	version
	database
	user
```



### 二、分组函数

```
	sum:求和
	avg:求平均值
	max:最大值
	min:最小值
	count:个数
	
特点：
	1.以上五个函数都忽略了null值，除了count(*)
	2.sum和avg适用于数值型，其他三个适合于任何类型的数据。
	3.都可以搭配distinct使用，用于统计去重后的结果
	4.count的参数可以支持：
		字段、*、常量值、一般放1
		建议使用count(*)
```



## 进阶5：分组查询

```
语法：
	select 分组函数,列（要求出现在group by后面的字段）
	from 表名
	where 筛选条件
	group by 分组的列表
	【order by 排序列表】
注意：
	查询列表必须特殊，要求是分组函数和group by后出现的字段
	
特点：
	1.可以按单个字段进行分组
	2.也可以按多个字段进行分组，之间用逗号隔开
	3.和分组函数一同查询的字段最好是分组后的字段
	4.分组筛选：
		
						针对的表		位置				关键词
		分组前筛选：		 原始表	  group by前		   	   where
		分组后筛选：	 分组后的结果集    group by后			having
	
	5.分组函数一定是在having子句中！
	6.可以支持排序
	7.having后可以支持别名
```

相关练习：

```sql
#分组查询
#简单的分组查询
#案例1：查询每个工种的最高工资
	SELECT MAX(salary),job_id FROM employees GROUP BY job_id;

#案例2：查询每个位置上的部门个数
	SELECT COUNT(*),`location_id`
	FROM `departments`
	GROUP BY location_id;


#添加筛选条件
#案例1：查询邮箱中包含a字符的每个部门的平均工资

	SELECT department_id,AVG(salary)
	FROM employees
	WHERE email LIKE '%a%'
	GROUP BY department_id;

#案例2：查询有奖金的每个领导手下员工的最高工资

	SELECT MAX(salary),manager_id,employee_id
	FROM employees
	WHERE commission_pct IS NOT NULL
	GROUP BY manager_id

#添加分组后的复杂的筛选条件
#案例1：查询哪个部门的员工个数大于2 

#① 查询每个部门的员工个数
	SELECT department_id,COUNT(*)
	FROM employees
	GROUP BY department_id;
#② 根据分组后的结果进行条件筛选
	SELECT department_id,COUNT(*)
	FROM employees
	GROUP BY department_id
	HAVING COUNT(*) > 2;


#案例2： 查询每个工种有奖金的员工的最高工资 >12000的工种编号和最高工资
# ① 查询出每个工种有奖金的员工的最高工资
	SELECT MAX(salary),job_id
	FROM employees
	WHERE commission_pct IS NOT NULL
	GROUP BY job_id;
# ② 根据分组后的结果进行条件筛选 最高工资> 12000
	SELECT MAX(salary),job_id
	FROM employees
	WHERE commission_pct IS NOT NULL
	GROUP BY job_id
	HAVING MAX(salary) > 12000;

#案例3：查询领导编号>102的每个领导手下的最低工资> 5000的领导编号，以及最低工资
#① 查询领导编号>102的每个领导手下的最低工资
	SELECT MIN(salary),manager_id
	FROM employees
	WHERE manager_id>102
	GROUP BY manager_id;
#② 根据分组结果将最低工资>5000的领导编号和最低工资查询出来
	SELECT MIN(salary),manager_id
	FROM employees
	WHERE manager_id>102
	GROUP BY manager_id
	HAVING MIN(salary) > 5000;

#按表达式或函数分组
#案例1：按员工姓名的长度分组，查询每一组的员工个数，筛选员工个数>5
#①按员工姓名的长度分组，查询每一组的员工个数
	SELECT COUNT(*),
	FROM employees
	GROUP BY LENGTH(last_name)
#② 员工个数>5
	SELECT COUNT(*)
	FROM employees
	GROUP BY LENGTH(last_name)
	HAVING COUNT(*) > 5;
```



## 进阶6：连接查询★★

一、笛卡尔积

```
笛卡尔乘积出现的原因：忽略了连接条件或无效
解决办法：添加有效的连接条件
现象：	一个表m条记录，另一个表n条记录  结果= m*n
```

二、连接查询的分类

```
 1.按年代分类
 	92sql:仅仅只支持内连接
 	99sql:支持内连接+外连接(左外连接和右外连接)+交叉连接【推荐使用】
 
 2.按功能分类
 	1.内连接
 		等值连接
 		非等值连接
 		自连接
 	2.外连接
 		左外连接
 		右外连接
 		全外连接
 	3.交叉连接
```

三、传统模式下——等值连接

```
特点：
	1.多个表等值连接的结果集是多个表的交集
	2.n个表等值连接需要n-1个连接条件
	3.可以搭配子句使用，比如order by ，group by
	4.等值连接，可以为表起别名
	5.多表的顺序无要求。
```

四、非等值连接

非等值连接是指除相等以外的连接。

非等值连接相关案例：

```sql
#案例1：查询员工的工资和工资级别
SELECT 
  salary,
  grade_level 
FROM
  employees e,
  job_grades g 
WHERE salary BETWEEN g.`lowest_sal` 
  AND g.`highest_sal` 
  AND g.`grade_level`='D' ;
```

五、自连接

就是连接自身表。

**自连接相关案例：**

```sql
#案例1:  查询员工名以及他的领导
select 
  e.last_name,
  e.employee_id,
  e.manager_id,
  m.`employee_id`,
  m.last_name 
from
  employees e,
  employees m 
where e.`manager_id` = m.`employee_id` ;
```

六、sql99语法：通过join关键字实现连接

```
sql99语法：
	select 查询列表
	from 表1 别名 【连接类型】 
	join 表2 别名
	on 连接条件
	【where 筛选条件】
	【group by 分组字段】
	【order by 排序字段】

连接分类分类：
	1.内连接:inner 
	2.外连接:
		左外连接：left 【outer】
		右外连接：right 【outer】
		全连接：full 
	3.交叉连接：cross

内连接的特点:
	1.inner可以省略
	2.连接条件放在on后面，筛选条件放在where后面
```



**通过join**关键字实现**内连接中的等值连接**

```sql
#sql99
#一、内连接
#1.等值连接
#案例1：查询员工名、部门名（调换位置）
SELECT 
  last_name,
  department_name 
FROM
  employees e 
  INNER JOIN departments d 
    ON e.`department_id` = d.`department_id`
    
#案例2：查询名字中包含e的员工名和工种名（添加筛选） 
    SELECT 
      last_name,
      job_title 
    FROM
      employees e 
      INNER JOIN jobs j 
        ON e.`job_id` = j.job_id 
    WHERE e.`last_name` LIKE '%e%' ;

#案例3：查询部门个数>3的城市名和部门个数
SELECT 
  city,
  COUNT(*) 
FROM
  departments d 
  INNER JOIN locations l 
    ON d.`location_id` = l.`location_id` 
GROUP BY city 
HAVING COUNT(*) > 3 ;

#案例4：查询哪个部门的员工个数>3的部门名和员工个数,并按个数降序
SELECT 
  COUNT(*),
  `department_name` 
FROM
  departments d 
  INNER JOIN employees e 
    ON d.`department_id` = e.`department_id` 
GROUP BY department_name 
ORDER BY COUNT(*) DESC ;

#案例5: 查询员工名、部门名、工种名【三表查询】
SELECT 
  last_name,
  department_name,
  job_title 
FROM
  employees e 
  INNER JOIN departments d 
    ON e.`department_id` = d.`department_id` 
  INNER JOIN jobs j 
    ON e.`job_id` = j.`job_id` ;
```

七、**通过join**关键字实现**外连接**

```
1.应用场景：
	用于查询一个表中有，另一个表中没有的记录
2.特点：
	1.外连接查询的结果为主表中所有的记录
		如过从表中有与主表匹配的记录，则显示记录
		如果从表中没有与主表匹配的记录，则显示为null
	2.左外连接，lfet outer join左边的表是主表
	3.右外连接，right outer join右边的表是主表
	4.左外和右外交换两个表的顺序，可以实现同样的效果。
```

相关案例：

![61371823158](C:\Users\ADMINI~1\AppData\Local\Temp\1613718231588.png)

```sql
#案例1：查询没有男朋友的女神名字
SELECT 
  g.name 
FROM
  beauty g 
  LEFT OUTER JOIN boys b 
    ON g.`boyfriend_id` = b.`id` 
WHERE b.`id` IS NULL ;

#案例2：查询哪个部门没有员工
SELECT 
  d.`department_name` 
FROM
  departments d 
  LEFT OUTER JOIN employees e 
    ON d.`department_id` = e.`department_id` 
    AND e.`employee_id` IS NULL ;
```



八：**通过join**实现**全连接**

全连接结果 =  内连接结果+表1中有但表2中没有的记录+ 表1中没有但表2中有的记录

九、通过**join**实现**交叉连接**

交叉连接：又称为笛卡尔积

```sql
SELECT 
  g.*,
  b.* 
FROM
  beauty g CROSS 
  JOIN boys b ;
```



连接查询相关练习题：

```sql
#一、查询编号>3 的女神的男朋友信息，如果有则列出详细，如果没有，用 null 填充
SELECT 
  g.`id`,
  g.`name`,
   b.*
FROM
  beauty g
  LEFT OUTER JOIN boys b 
    ON g.`boyfriend_id` = b.`id` 
WHERE g.`id` > 3 ;


#二、查询哪个城市没有部门
SELECT 
  city,
  d.`department_id` 
FROM
  locations l 
  LEFT OUTER JOIN departments d 
    ON l.`location_id` = d.`location_id` 
WHERE d.`department_id` IS NULL ;


#三、查询部门名为 SAL 或 IT 的员工信息
SELECT 
  e.*,
  d.department_name 
FROM
  departments d 
  LEFT OUTER JOIN employees e 
    ON e.`department_id` = d.`department_id` 
WHERE d.`department_name` = 'SAL' 
  OR d.`department_name` = 'IT' 
```



## 进阶7：子查询★★★★

含义：在其他语句中嵌套select查询语句，其中被嵌套的select语句，就称为子查询或内查询

在外面的查询语句，称为主查询或外查询。

分类：

```
1.按子查询出现的位置分类：
	select后面
		仅仅支持标量子查询
	from后面
		支持表子查询
	where/having后面 ★★
		标量子查询（单行）
		列子查询（多行）
		
		行子查询
	exists后面（相关子查询）
		表子查询
2.按结果集的行列数不同分类：
	标量子查询（结果集只有一行一列）
	列子查询（结果集是一列多行）
	行子查询（结果集是一行多列）
	表子查询（结果集一般是多行多列）
```

特点：

```
	1.子查询放在小括号内
	2.子查询一般放在条件的右侧
	3.标量子查询一般搭配着单行操作符使用
		> < = <> >= <= 
	4.列子查询，一般搭配着多行操作符使用
		in ,any/some ,all
```

标量子查询的相关练习：

```sql
#案例1：谁的工资比Abel高？
#①.查询Abel工资
SELECT 
  salary 
FROM
  employees 
WHERE last_name = 'Abel' 
#②.查询员工的信息,满足salary > ①的结果
  SELECT 
    * 
  FROM
    employees 
  WHERE salary > 
    (SELECT 
      salary 
    FROM
      employees 
    WHERE last_name = 'Abel')
    
    
#案例2：返回job_id和141号员工相同，salary比143号员工多的员工姓名，job_id和工资
#① 查询141号员工的job_id
SELECT job_id FROM employees WHERE employee_id = 141;
#② 查询43号员工工资
SELECT salary FROM employees WHERE employee_id = 143;
# 查询员工姓名，job_id和工资，要求job_id=①，salary > ②
SELECT last_name,job_id,salary
FROM employees
WHERE job_id = (SELECT job_id FROM employees WHERE employee_id = 141)
AND salary >(SELECT salary FROM employees WHERE employee_id = 143); 


#案例3：返回公司工资最少的员工last_name,job_id和salary【使用到了分组函数】
#①：查询工资最少的员工
SELECT MIN(salary) FROM employees;
#②：查询last_name,job_id和salary，要求salary = ①
SELECT last_name,job_id,salary FROM employees WHERE salary =(SELECT MIN(salary) FROM employees);


#案例4：查询最低工资大于50号部门最低工资的部门id和其最低工资
#①查询每个部门最低工资
SELECT MIN(salary),department_id FROM employees GROUP BY department_id;
#②查询50号部门最低工资
SELECT MIN(salary) FROM employees WHERE department_id = 50;
#③查询部门id和其最低工资
SELECT MIN(salary),department_id FROM employees GROUP BY department_id HAVING MIN(salary) >(SELECT MIN(salary) FROM employees WHERE department_id = 50) ;

```



where后面的列子查询和行子查询相关练习：

```sql
#2.列子查询（多行子查询）
#案例1：返回location_id是1400或1700的部门中的所有员工姓名
#① 查找location_id是1400或1700的部门编号
SELECT department_id  FROM departments WHERE location_id = 1400 OR location_id = 1700
#② 查询员工姓名，要求部门号是①中的某一个
SELECT last_name FROM employees WHERE department_id IN (

SELECT department_id 
FROM departments
WHERE location_id = 1400 OR location_id = 1700

);

#案例2：返回其他部门中比job_id为"IT_PROG"部门任一工资低的员工的工号、姓名、job_id和salary
#①：查询job_id为"IT_PROG"部门任一工资
SELECT DISTINCT salary FROM employees WHERE job_id ='IT_PROG';
#②：查询工号、姓名、job_id和salary
SELECT job_id,last_name,salary
FROM employees
WHERE salary < ANY(

SELECT DISTINCT salary 
FROM employees
WHERE job_id ='IT_PROG'
);


#案例3: 返回其他部门中比job_id为"IT_PROG"部门所有工资低的员工的工号、姓名、job_id和salary
SELECT job_id,last_name,salary
FROM employees
WHERE salary < ALL(

SELECT DISTINCT salary 
FROM employees
WHERE job_id ='IT_PROG'
);


#3.行子查询（结果集一行多列或多行多列）
#案例：查询员工编号最小并且工资最高的员工信息
#① 查询编号最小的员工
SELECT MIN(employee_id) FROM employees
#② 查询工资最高的员工
SELECT MAX(salary) FROM employees;
#③ 查询员工信息
SELECT 
  * 
FROM
  employees 
WHERE employee_id = 
  (SELECT 
    MIN(employee_id) 
  FROM
    employees) 
  AND salary = 
  (SELECT 
    MAX(salary) 
  FROM
    employees) ;
    
#方式二：使用行子查询
SELECT 
  * 
FROM
  employees 
WHERE (employee_id, salary) IN 
  (SELECT 
    MIN(employee_id),
    MAX(salary) 
  FROM
    employees)
```

from后面的子查询相关练习题:

```sql
#三、from后面的子查询 
#案例：查询每个部门的平均工资的工资等级 
#① 查询每个部门的平均工资
SELECT 
  AVG(salary),
  department_id 
FROM
  employees 
GROUP BY department_id 
#② 查询工资等级 
  SELECT 
    ag_dep.*,
    g.`grade_level` 
  FROM
    (SELECT 
      AVG(salary) avgsal,
      department_id 
    FROM
      employees 
    GROUP BY department_id) ag_dep 
    INNER JOIN job_grades g 
      ON ag_dep.avgsal BETWEEN g.`lowest_sal` 
      AND g.`highest_sal` 
```

exists后面的子查询：

```
exists语法：
exists(完整的查询语句)
结果：
1或0
```

## 进阶8：分页查询★★

应用场景：当要显示的数据，一页显示不全，需要提交分页请求



```
语法:
select 分组函数,列（要求出现在group by后面的字段）
	from 表名
	where 筛选条件
	group by 分组的列表
	【order by 排序列表】
	limit 【offset】,size
	
offset:要显示条目的起始索引（起始索引从0开始）
size：要显示的条目个数

特点：
	1.limit语句放在查询语句的最后
	2.公式
	要显示的页数 page,每页的条目数为size
	
	select 查询列表
	from 表
	limit (page-1)*size,size;
	
	size = 10
	page  offset
	 1		0
	 2		10
	 3		20
	 
	 offset = (page-1)*size

```



## 进阶9： 联合查询

含义：将多个查询结果合并成一条结果

```
语法：
	查询语句1
	union
	查询语句2
	union
	....

应用场景：要查询的结果来自于多个表，且表与表之间没有直接的联系关系，但是查询的信息一致时

特点：
	1.union联合查询时，列名的数量必须一致！
	2.union联合查询时，多个表之间的字段的顺序需要保持一致！
	3.union联合查询自动去重，如果不想去重使用union all 
```



# 5.DML语言的学习

## 插入

**语法**：

​	方式一：insert into 表名(列名1，列名2,....)  values(值1，值2，....)

​	方式二：insert into 表名 set 列名1= 值1，列名2=值2，.....

**两种插入方式的比较**：

​	1.方式一可以批量插入 ，方式二不行

​	2.方式一可以搭配子查询使用，方式二不行

插入的**特点**:

```
1、字段类型和值类型一致或兼容，而且一一对应
2、可以为空的字段，可以不用插入值，或用null填充
3、不可以为空的字段，必须插入值
4、字段个数和值的个数必须一致
5、字段可以省略，但默认所有字段，并且顺序和表中的存储顺序一致
```



## 修改

**修改单表语法：**

​	update 表名 set 列名=值1，列名=值2   【where】

**修改多表**语法：

```
sql92语法：
	update 表1 别名，表2 别名
	set 字段=值1，字段=值2
	where 连接条件
	and 筛选条件
	
sql99语法：
	update 表1
	left|right|inner join 表2
	on 连接条件
	set 列1=值1，列2=值2
	【where 筛选条件】
	
	
#修改多表记录
#案例1：修改男朋友是张飞的女神们的手机号为114
UPDATE beauty b
INNER JOIN boys bo
ON b.`boyfriend_id` =bo.`id`
SET phone='114'
WHERE b.`boyfriend_id` = 2;
```



## 删除

**方式1：delete语句**

单表删除：

​	delete from 表名 where 筛选条件



多表删除：

```
sql92语法
delete 要删除哪个表的数据就填哪个表的别名
from 表1 别名，表2 别名
where 连接条件
and 筛选条件

sql99语法
delete 要删除哪个表的数据就填哪个表的别名
from 表1 别名
inner|left|right join 表2 别名
on 连接条件
where 筛选条件

练习：
#删除多表数据
#案例：删除黄晓明以及他女朋友的信息
DELETE b,bo
FROM beauty b
INNER JOIN boys bo 
ON b.`boyfriend_id` = bo.`id`
WHERE bo.`boyName`='黄晓明'
```



**方式2：truncate语句**

```
truncate table 表名
```



**truncate和delete**的**区别**【**面试题**】

```
1.delete删除可以添加where条件，truncate不可以
2.truncate删除的效率稍微比delete高一点。
3.delete删除可以回滚，truncate删除不可以回滚。
4.truncate删除没有返回值，delete删除有返回值
5.delete删除带自增长的列后，在添加记录时，自增长列会从断点处继续自增
  truncate删除带自增长的列后，在添加记录时，自增长的列会从1重新开始
```



# 6.DDL语言的学习

## 库和表的管理

库的管理：

```
一、创建库
create database  if not exists 库名

二、修改库
修改库只能修改库的字符集。
alter database 库名 character set 字符集

二、删除库
drop database if exists 库名
```



表的管理：

​	1.创建表

```
语法：
create table 表名(
	字段 字段类型 约束,
	字段 字段类型 约束,
	字段 字段类型 约束,
	...
	字段 字段类型 约束
);
```

​	2.修改表

```
语法：
	alter table 表名 add|drop|modify|change column 字段名【字段类型/约束】
	
#①添加新列
ALTER TABLE book ADD COLUMN pubDate DATETIME ;

#②修改字段名
ALTER TABLE author CHANGE author_name authorName VARCHAR(20);

#③修改字段类型
ALTER TABLE book MODIFY pubDate TIMESTAMP;

#④删除字段
ALTER TABLE author DROP COLUMN salary;

#⑤修改表名
ALTER TABLE author RENAME TO book_author ;
```

​	3.删除表

```
语法：
	drop table 【if exists】  表名
```

​	**4.复制表★★**

```
#仅仅只复制表的结构
CREATE TABLE copy LIKE book_author;

#复制表的结构及所有数据
CREATE TABLE copy2 
SELECT * FROM book_author;

#复制部分数据
CREATE TABLE copy3 
SELECT id,nation
FROM book_author 

#复制表，不带数据
CREATE TABLE copy4 
SELECT * FROM book_author
WHERE 1=2;
```



## 常见类型

```
整型：tinyint,smallint,meduimint,int/integer,bigint
		1		2		3		  4			8字节

小数：
	浮点型：
		1.float
		2.double
	定点型

字符型：
	1.较短的文本：	写法		M的意思	 			特点  	空间的耗费 			效率
		char		char(M)		最大字符数，可以省略   长度固定     比较耗费			 高
		varchar		varchar(M)	 最大字符数，不可以省略  长度可变	  相对节省			相对较低
	2.较长的文本：
		text
		blob（较大的二进制）
日期型：
	1.date
	2.year
	3.time
	4.datetime：4个字节，不受时区影响
	5.timestamp：8个字节，受时区影响
	
blob类型：
```



## 常见约束

**含义**：一种限制，用于限制表中的数据，保证表中数据的真实和可靠。

**分类**：六大约束

​		NOT NULL:非空，用于保证该字段的值不能为空

​		DEFAULT:默认，保证该字段有默认值

​		PRIMARY KEY :主键，保证该字段的值 **唯一且非空**

​		UNIQUE:唯一，保证该字段的值唯一

​		CHECK:检查约束，mysql不支持，用于保证该字段的值正确。

​				比如 性别：只允许输入男或女

​		FOREIGN KEY:外键约束，限制两个表之间的关系，用于保证该字段的值必须来自于主表的关联列的值。

​					**在从表添加外键约束**。



**添加约束的时机**：

​				1.在创建表时

​				2.在修改表时（还未有数据）

**约束添加时的分类**：

​				1.列级约束

​					六大约束语法上都支持，但**外键约束**不起效果。

​				2.表级约束

​					PRIMARY KEY，UNIQUE，CHECK，FOREIGN KEY



创建表时添加**列级约束：**

```
语法：
直接在字段名和字段类型后面追加约束类型

#1.添加列级约束
CREATE TABLE stuinfo(

	id INT PRIMARY KEY,#主键
	stuName VARCHAR(20) NOT NULL,#非空
	gender CHAR(1) CHECK(gender IN ('男','女')),# 检查约束
	seat INT UNIQUE,#唯一约束
	age INT DEFAULT 18,#默认约束
	majorId INT REFERENCES major(id)#外键约束

);

CREATE TABLE major(

	id INT PRIMARY KEY ,
	major_name VARCHAR(20) 

);
```

查看某个表的索引

```sql
show index from 表名
```



创建表时添加**表级约束**：

```
语法：在各个字段的最下面
【constraint 约束名】 约束类型(约束字段);


#2.添加表级约束
CREATE TABLE stuinfo(

	id INT ,
	stuName VARCHAR(20), 
	gender CHAR(1) ,
	seat INT ,
	age INT ,
	majorId INT ,
	
	CONSTRAINT pk PRIMARY KEY(id),
	CONSTRAINT uq UNIQUE(seat),
	CONSTRAINT fk_sutinfo_major FOREIGN KEY(majorId) REFERENCES major(id)

);
```



**外键的特点**：

```
1.设置外键时需要在从表设置
2.设置外键时，主表的关联列需要是一个key
3.外键设置好之后，在添加数据时需要先添加主表的数据，在添加从表数据
	删除数据时，需要先在从表删除数据，在从主表删除数据
4.外键字段的字段类型必须与主表中关联列中的字段类型一致或兼容。
```



**★★★修改表时添加约束：**

```
1.添加列级约束
alter table 表名 modify column 字段名 字段类型 约束类型;

2.添加表级约束
alter table 表名 add 【constraint 约束名】 约束类型(字段名);


#1.添加主键约束
#①：添加列级约束
ALTER TABLE stuinfo MODIFY COLUMN id INT PRIMARY KEY ;
#②：添加表级约束
ALTER TABLE stuinfo ADD PRIMARY KEY(id);

#2.添加唯一约束
ALTER TABLE stuinfo MODIFY COLUMN seat INT UNIQUE;

#3.添加外键约束
ALTER TABLE stuinfo ADD CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorId) REFERENCES major(id);

```



★★★修改表时**删除**约束：

```
语法： alter table 表名 drop index 约束名
	  alter table 表名 drop primary key ；
	  
#1.删除外键
ALTER TABLE stuinfo DROP FOREIGN KEY fk_stuinfo_major;

#2.删除主键
ALTER TABLE stuinfo DROP PRIMARY KEY ;

#3.删除唯一约束
ALTER TABLE stuinfo DROP INDEX seat;
```



## 标识列

含义：标识列，又称为自增长列，可以不用手动的插入值，系统提供默认的序列值。



**特点**：

​	1.标识列必须要和一个key搭配

​	2.一个表至多1个标识列

​	3.标识列的类型只能是数值型

​	4.标识列可以通过 set auto_increment_increment=3；设置步长

​	5.可以通过手动插入值，设置起始值。



# 7.TCL(事务控制语言)

## 特点

```
(ACID)
原子性：一个事务是不可再分的最小单位，要么全部执行，要么都不执行。
一致性：数据库中的数据在一个事务操作前后的状态保持一致
隔离性：一个事务不受其他事务的干扰。
持久性：一个事务一旦提交，则会永久的改变数据库中的数据。
```

## 事务的使用步骤

```
1.开启事务
2.在事务中编写一组sql
3.提交事务/回滚事务


#1.开启事务
SET autocommit = 0;
#2.sql
UPDATE account SET balance=1000 WHERE NAME='张无忌';
UPDATE account SET balance=1000 WHERE NAME='赵敏';
#提交事务
#回滚事务
#rollback;
COMMIT;
```

## 事务的分类

```
1.隐式事务
	insert、update、delete语句本身就是一个事务
2.显示事务：具有明显的开启事务、提交事务的标志
	显示事务使用到的关键字
	
set autocommit = 0;
【start transaction;】
commit;
rollback;

savepoint  断点
commit to 断点
rollback to 断点
```

## 事务的隔离级别

事务并发问题何时发生?

```
当多个事务同时操作同一个数据库的相同数据时
```

事务的并发问题有哪些？

```
脏读：一个事务读取到另一个事务未提交的数据
不可重复读：同一个事务中，多次读取到的事务不一致
幻读：一个事务在读取数据时，另一个事务正在更新，导致第一个事务读取到了没有更新的数据。
```

如何避免事务的并发问题？

```
READ UNCOMMITTED
READ COMMITTED 可以避免脏读
REPEATABLE READ 可以避免脏读、不可重复读和一部分幻读
SERIALIZABLE 可以避免脏读、不可重复读、幻读、
```

查看隔离级别:

```
select @@tx_isolation ;
```

设置当前隔离级别：

```
set session|global transation isolation level 隔离级别名;
```



Mysql默认隔离级别：

```
REPEATABLE READ
```

oracle默认隔离级别：

```
READ COMMITTED;
```



# 8.视图

含义：虚拟表，和普通表一样使用

## 创建视图

```
语法：
create view 视图
as
查询语句;
```

## 修改视图

```
方式一：
create or replace view 视图名
as
查询语句;

方式二：
alter view 视图名
as 
查询语句；
```

## 删除视图

```
语法:
drop view 视图名，视图名2
```

## 查看视图结构

```
方式一：
desc 视图名;
方式二：
show create view 视图名
```

## 某些视图不能更新

```
包含以下关键词的sql：分组函数、distinct、group by、having、union或union all
常量视图
select中包含子查询
join
from一个 不能更新的视图
where子句的子查询引用了from子句中的表
```



# 9.变量

```
变量的分类：
	1.系统标量
		(1)全局变量
		(2)会话变量
	2.自定义变量
```

## 系统变量

含义：变量是由系统提供的，不是用户定义的，属于服务器层面

一、全局变量

作用域：针对于所有会话（连接）有效，但不能跨重启

```
语法：
1.查询所有的系统变量
show global|【session】  variables 

	global:全局
	sesion:会话
2.查看所有的会话系统变量
show 【session】 variables

3.查看指定的某个系统变量的值
select @@global|【session】.系统变量名

4.为某个系统变量赋值
方式一：
set global|【session】 系统变量名 = 值;
方式二：
set global|【session】.系统变量名 =值1;


```



二、会话变量

作用域：针对于当前会话（连接）有效

```
1.查看所有的会话系统变量
show 【session】 variables

2.查看指定的某个会话系统变量的值
select @@【session】.系统变量名

3.为某个会员系统变量赋值
方式一：
set 【session】 系统变量名 = 值;
方式二：
set 【session】.系统变量名 =值1;
```



## 自定义变量

说明：变量是用户自定义的，不是由系统提供的

一、**用户变量**

作用域：针对于当前会话（连接）有效

应用在begin end内 或者外都行。

```
1.声明并初始化   select声明变量时 只能使用 :=
set @变量名 = 值
set @变量名 := 值
select @变量名 := 值

2.赋值
set @变量名 = 值
set @变量名 := 值
select @变量名 ：= 值

方式二：一般用于将表中的值赋给变量
select 字段名|表达式 into 变量
from 表名;

3.查看变量值
select @变量名
```



二、**局部变量**

作用域：仅仅在定义它的begin end中有效

**应用在begin end中的第一句话！！**

```
1.声明
declare 变量名 变量类型;
declare 变量名 变量类型 default 值；

2.赋值
declare 局部变量名 = 值;
declare 局部变量名 := 值;
select  @局部变量名 ：= 值;

方式二：通过select into
	select 字段名 into 局部变量名
	from 表名;

3.使用
select 局部变量名;
```



# 10.存储过程

含义：是一组预先编译好的sql的集合，理解成批处理语句

好处：

​	1.提高代码的重用性

​	2.简化操作

​	3.减少了编译次数并且减少了对数据库的连接次数，提高了效率

一、创建存储过程

```
语法：
	create procedure 存储过程名称(参数列表)
	begin
			存储过程体（一组有效合法的sql语句）
	end
	
注意：
	1.参数列表包含三部分
	参数模式	参数名		参数类型
举例：
	in	stuname	varchar(20)
	
参数模式：
	in:该参数可以作为输入，在调用方调用时需要传入参数值
	out:该参数可以作为输出，也就是该参数可以作为返回值
	inout:该参数既可以作为输入也可以作为输出。也就是该参数既需要传入值，又可以返回值。
	
如果存储过程体仅仅只有一句话，begin end可以省略
存储过程体中的每条sql语句都必须以分号结尾
存储过程的结尾可以使用delimiter重新设置
语法：
	delimiter 结束标记
案例：
	delimiter ￥；
```

二、调用存储

```
语法：
	call 存储过程名(实参列表);
```



三、相关案例

​	1.空参的存储过程

```
#1.空参列表
#案例：插入到admin表中5条记录
DELIMITER $
CREATE PROCEDURE myp1()
BEGIN

	INSERT INTO admin(username,`password`) 
	VALUES('rose','0000'),('lilei','0000'),('jack','0000'),('tom','0000'),('bob','0000');
END $

CALL myp1();

SELECT * FROM admin;
```



​	2.带in模式参数的存储过程

```
#2.创建带in模式参数的存储过程
#案例1：创建存储过程，实现 根据女神名，查询对应的男生名

DELIMITER $
CREATE PROCEDURE myp2(IN beautyName VARCHAR(20))
BEGIN
	SELECT bo.*
	FROM boys bo 
	RIGHT OUTER JOIN beauty b 
	ON bo.id = b.`boyfriend_id`
	WHERE b.`name`= beautyName;
END $

CALL myp2('周芷若');


#案例2:创建存储过程，实现 检查用户是否登录成功
DELIMITER $
CREATE PROCEDURE myp3(IN username VARCHAR(20),IN PASSWORD VARCHAR(20))
BEGIN
	DECLARE result INT DEFAULT 0; #声明变量并初始化

	SELECT COUNT(*) INTO result  #给变量赋值
	FROM admin
	WHERE admin.`username` = username
	AND admin.`password` =PASSWORD;
	
	SELECT IF(result>0,'登录成功','登录失败'); #调用变量	
END

CALL myp3('张飞','123');
```



​	3.带out模式参数的存储过程

```
#3.创建带out模式的存储过程

#案例1：根据女神名返回对应的男神名
DELIMITER $
CREATE PROCEDURE myp5(IN beautyname VARCHAR(20),OUT boyname VARCHAR(20))
BEGIN
	
	SELECT bo.`boyName` INTO boyname
	FROM boys bo
	INNER JOIN beauty b 
	ON bo.id = b.`boyfriend_id`
	WHERE b.name = beautyname;

END$

CALL myp5('王语嫣',@bname);

SELECT @bname;

#案例2：根据女神名，返回对应的男神名和魅力值
DELIMITER $
CREATE PROCEDURE myp6(IN beautyname VARCHAR(20),OUT boyname VARCHAR(20),OUT usercp INT)
BEGIN
	
	SELECT bo.`boyName`,bo.userCP INTO boyname,usercp
	FROM boys bo
	INNER JOIN beauty b 
	ON bo.id = b.`boyfriend_id`
	WHERE b.name = beautyname;

END$

CALL myp6('周芷若',@bname,@usercp);

SELECT @bname,@usercp;
	
```



​	4.带inout模式参数存储过程

```
#4.带inout模式参数的存储过程
#案例：传入a和b的值，最终返回a和b的翻倍。
DELIMITER $
CREATE PROCEDURE myp7(INOUT a INT,INOUT b INT)
BEGIN

	SET a = a*2;
	SET b =b* 2;

END

# 定义变量
SET @m=10;
SET @n=20;
CALL myp7(@m,@n);

SELECT @m,@n;

```

## 删除存储过程

```
语法：
	drop procedure 存储过程名
```



## 查看存储过程的结构

```
语法：
	show create procedure 存储过程名
```





# 11.函数

和存储过程类似，但是有区别

```
函数与存储过程的区别：
	存储过程：返回值可以有0个，也可以有多个，适合批量插入、批量删除
	函数：返回值有且仅有一个，适合数据处理后 返回一个结果
```



## 创建以及调用函数

```
创建函数语法
create function 函数名 (参数列表)  returns 返回类型
begin
		函数体
		
		return 值
end

注意：
	1.参数列表 包含两部分：
		参数名	  参数类型
	2. 函数体中必须要有return语句，否则会报错
	3.return语句原则上放在函数体哪都不报错，但是建议放在函数体的最后。
	4.如果函数体只有一行语句，begin end可以省略
	5.使用delimiter语句设置结束标记
```



## 调用函数

```
语法:
	select 函数名(参数列表);
```



## 函数相关案例

```sql
#1.无参有返回
#案例：返回公司的员工个数

DELIMITER $
CREATE FUNCTION myf1() RETURNS INT
BEGIN

	DECLARE c INT DEFAULT 0; #声明一个变量
	SELECT COUNT(*) INTO c #为变量赋值
	FROM employees;
	
	RETURN c; #返回值
END$

SELECT myf1()$;


#2.有参有返回值
#案例1:根据员工名返回它的工资
DELIMITER $
CREATE FUNCTION myf2(empName VARCHAR(20)) RETURNS DOUBLE 
BEGIN
	SET @sal = 0;
	SELECT salary INTO @sal
	FROM employees 
	WHERE last_name =empName;
	RETURN @sal;
END$

SELECT myf2('Bruce');

#案例2:	根据部门名，返回该部门的平均工资
DELIMITER $

CREATE FUNCTION myf3(deptName VARCHAR(20)) RETURNS DOUBLE
BEGIN
	
	DECLARE asal DOUBLE DEFAULT 0;
	
	SELECT AVG(salary) INTO asal
	FROM employees e 
	INNER JOIN departments d
	ON e.`department_id` = d.`department_id`
	WHERE d.`department_name` = deptName;
	
	RETURN asal;
	
END$

SELECT myf3('IT');
```



## 查看函数

```
语法：
	show create function 函数名;
```



## 删除函数

```
语法：
	drop function 函数名;
```





# 12.流程控制结构

## 流程控制结构分类

```
顺序结构：程序从上往下依次执行
分支结构：程序从两条或多条路径中选择一条执行
循环结构：程序在满足一定条件的基础上，重复执行一段代码
```



## 一、分支结构

1.IF函数

```
1.if函数
功能：实现简单的双分支
语法：
	if(表达式1，表达式2，表达式3)
	执行顺序：
	如果表达式1成立，则返回表达式2的值，否则返回表达式3的值
应用：任何地方

```



**2.case语句**

```
情况一： 类似于java中的switch
case 变量|表达式
when  值1  then 结果1或者语句1(如果是语句，需要加分号)
when  值2  then 结果2或者语句2(如果是语句，需要加分号)
...
else 结果n或者语句n
end 【case】 (如果放在begin end中需要加case，如果放在select后面不需要)

情况二：类似于多重If
case
when  条件1  then 结果1或者语句1(如果是语句，需要加分号)
when  条件2  then 结果2或者语句2(如果是语句，需要加分号)
...
else 结果n或者语句n
end 【case】 （如果是放在begin end中需要加case，如果放在select后面不需要）

```

case语句相关案例：

```sql
#案例：创建存储过程，传入一个分数，判断其所在等级 
#比如 90-100 显示A  80-90 显示B 60-80 显示C  否则显示D

DELIMITER $
CREATE PROCEDURE test_case(IN score INT)
BEGIN

	CASE 
	WHEN score >=90 AND score <=100 THEN SELECT 'A';
	WHEN score >=80 THEN SELECT 'B';
	WHEN score >=60 THEN SELECT 'C';
	ELSE SELECT 'D';
	END CASE;
END$

CALL test_case(30);
```

**3.if elseif语句**

```
语法：
	if 条件1 then 语句1；
	elseif 条件2 then 语句2；
	...
	else 语句n;
	end if ;
特点：只能应用在begin end中！！！！
```



if elseif相关案例：

```sql
#案例：创建函数，传入一个分数，返回其所在等级 
#比如 90-100 返回A  80-90 返回B 60-80 返回C  否则返回D
DELIMITER $
CREATE FUNCTION test_ifelseif(score INT) RETURNS CHAR
BEGIN

	IF score >=90 AND score <=100 THEN RETURN 'A';
	ELSEIF score >=80 THEN RETURN 'B';
	ELSEIF score >=60 THEN RETURN 'C';
	ELSE RETURN 'D';
	END IF ;

END$

SELECT test_ifelseif(30);

```



## 二、循环结构

### 循环结构分类

```
循环结构：
while、loop、repeat
循环结构 一定要放在begin end 里面！！！！！

循环控制语句： 
	iterate：相当于java中的continue，跳出本次循环，继续执行下一个循环。
	leave：相当于java中的break;跳出循环。
```

### while循环

```
语法：
	【标签：】 while 循环条件 do
			循环体;
	end while 【标签】;
```

while相关案例：

```
#没有添加循环控制语句
#案例1：批量插入，根据设置的次数插入多条记录到admin表中
DROP PROCEDURE p1;
DELIMITER $
CREATE PROCEDURE p1(IN insertCount INT)
BEGIN
	#声明 初始化变量
	DECLARE i INT DEFAULT 1;
	# 循环
	WHILE i<=insertCount DO 
		INSERT INTO admin(username,`password`) 
		VALUES(CONCAT('john',i),'0000');
		
		SET i = i+1;
	END WHILE;

END$
CALL p1(20);

#案例2:添加level语句，
##案例1：批量插入，根据设置的次数插入多条记录到admin表中 >20 停止插入
DELIMITER $
CREATE PROCEDURE p2(IN insertCount INT)
BEGIN

	DECLARE i INT DEFAULT  1;
	a:WHILE i <= insertCount DO 
		INSERT INTO admin(username,`password`) 
		VALUES(CONCAT('xiaohua',i),'0000');
		IF i > 20 THEN LEAVE a;
		END IF;
		SET i = i+1;
	END WHILE a;
END$

CALL p2(50);
```



### loop循环

```
语法：
	【标签：】 loop
				循环体;
	end loop 【标签】;
	
	
该循环可以模拟一个死循环
```



### repeat循环

```
语法：
	【标签：】 repeat
			循环体;
	until 结束循环的条件
	end repeat【标签】;
```





