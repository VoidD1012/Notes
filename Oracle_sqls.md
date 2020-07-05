```sql
/*
where子句中的运算符
注意：where子句中操作的数据必须是实际存在于数据表中的字段，虚拟表中的数据要交给having处理
*/
-- !=、<> 不等于
select * from emp where deptno != 20;
select * from emp where deptno <> 20;
-- <、>、<=、>=
select sal from emp where sal < 1500;
select sal from emp where sal > 2999;
select sal from emp where sal <= 1500;
select sal from emp where sal >= 3344;
--any()
--取其中某一个值
select sal from emp where sal > any(1000, 1500, 3000);
--some()
--some和any是同一个效果，只要大于其中某一个值都会成立
select sal from emp where sal > some(500, 800, 3000);
--all()
--所有的值都必须被满足
select sal from emp where sal > all(500, 1000, 1500);

--is null, is not null
--在sql语法中，null表示一个特殊的含义，null != null，想判断null，不能使用=、!=判断，需要使用is、is not来判断
select * from emp where comm = null;(错)
select * from emp where comm is null;(对)
select * from emp where null = null;(无结果)
select * from emp where null is null;

--between x and y
--范围,包含x和y的值
select * from emp where sal between 1500 and 3000;
select * from emp where sal >= 1500 and sal <= 3000;

--需要进行某些值的等值判断的时候可以使用in和not in
--in(list)  /  in(sub-query)
select * from emp where deptno in(10, 20);
select * from emp where deptno = 10 or deptno = 20;
select ename, sal from emp where deptno in (select deptno from dept where dname ='SALES' or dname = 'SEARCH');
--在sql语句中可以使用and和or这样的关键字，and相当于是与操作，or相当于是或操作
--当and和or出现在同一个sql语句中，此时需要注意and和or的优先级：and的优先级要高于or，所以一定为or的相关操作加上（）
--not in(list)
select * from emp where deptno not in(10,20);
select * from emp where deptno != 10 and deptno != 20;

--exists(sub-query)
--sub-query：子查询，一定是一个完整的sql语句
--现在要查询部门编号为10和20的员工，要求使用exists实现
select * from emp where deptno = 10 or deptno = 20;
--当exists中的子查询语句能查询到对应结果的时候，意味着条件满足，相当于双层for循环
--也就是说，外层的select语句会遍历表中的每一条数据，在遍历时，会对每一条数据进行判断，而判断的依据就是内层子查询的结果，如果有结果，那么判断这条数据满足查询条件，放入结果集；如果子查询没有结果，那么判断这条数据不满足查询条件，不放入结果集。
select * from emp e where exists(select e.deptno from dept where e.deptno = 10 or e.deptno = 20);
--所以需要通过外层循环来规范内层循环
select * from emp e where exitst(select deptno from dept d where (d.deptno = 10 or d.deptno = 20) and e.deptno = d.deptno);
--like 模糊查询
--在like语句中，需要使用占位符或者通配符
--  _ :某个字符或者数字仅出现一次
--  % :任意字符出现任意次数
--查询名字以S开头的用户
select * from emp where ename like('S%');
--查询名字的首字母是S，倒数第二个字符为T的用户
select * from emp where ename like('S%T_');
--显示名字中带%的用户
--escape(转义字符)对特殊符号进行转义
select * from emp where ename like('%.%%') escape('.');
--使用like的时候要慎重，因为like的效率比较低
--使用like可以参考使用索引，但是要求不能以%开头，因为这样是最慢的方式
--涉及到大文本的检索的时候，可以使用某些框架luence、solr等进行检索，尽量不要通过like来检索

--order by进行排序
--默认情况下完成的是升序的操作，asc:默认的排序方式，表示升序；desc：降序
--排序是按照自然顺序进行排序，数字按大小，字符串按照字典序排序
--在进行排序的时候可以指定多个字段，而且多个字段可以使用不同的排序方式
--每次在执行order by的时候相当于是做了全排序，全排序很消耗内存，一定要在业务不太繁忙的时候进行，特别是一些定时任务，防止对正常业务产生影响
select * from emp order by sal;
select * from emp order by sal desc;
select * from emp order by sal desc, ename asc;

--计算字段
--sql允许select子句中出现+，-，*，/以及列名和常数的表达式
--拼接字段：||和+，首选||（mysql中||表示or，一般用concat()）
--字符串连接符
select 'my name is '||ename name from emp;
select concat('my name is ', ename) from emp;
--dual是Oracle数据库中的一张虚拟表，没有实际的数据，可以用来做测试
select 100+null from dual;--和null的运算都得null
--计算所有员工的年薪
	--有些员工的comm为null，所以下面的语句不能得到正确结果集，应当要将null进行转化
select ename, (e.sal+e.comm)*12 from emp e;（错）
	--引入函数nvl(arg1,arg2),如果arg1为null，那么返回arg2；如果不是null，则返回原来的值
select ename, (e.sal+nvl(e.comm,0))*12 from emp e;

--结果集的集合运算
--A
select * from emp where deptno = 30;
--B
select * from emp where sal > 1000;
	--union 并集
	A union B
	--union all 全集=并集+交集
	A union all B
	--intersect 交集
	A intersect B
	--minus 差集
	A minus B
```







### Oracle函数



##### 1.组函数\聚合函数:对多行数据进行操作，并返回一个单一的结果

**组函数仅可在选择列表或查询的having子句中使用**

```sql
--组函数一般和group by一起使用
--常见的5个组函数：
--avg()：平均值
select avg(sal) from emp;
--min()：最小值
select min(sal) from emp;
--max()：最大值
select max(sal) from emp;
--sum()：总和
select sun(sal) from emp;
--count()：计数
select count(sal) from emp;

--group by 按照某些相同的值去进行分组操作。可以指定一个列或者多个列，但是当使用了group by之后，选择列表中只能包含组函数的值或者group by的普通字段
--having子句：用来对虚拟表进行条件查询返回结果
--求每个部门的平均薪水
select avg(sal) from emp group by deptno;
--求平均薪水大于1500的部门
select avg(sal) from emp group by deptno having avg(sal) > 2000;
select avg(sal),deptno,ename from emp group by deptno having avg(sal) > 2000(错)

```



##### 单行函数：输入单个数值，返回一个值

```sql
1.字符函数：全以字符作为参数，返回值分为两类：一类返回字符值，一类返回数字值

--concat:表示字符串的连接，等同于 ||
select concat('my name is ', ename) from emp;
--initcap:将字符串的首字母大写
select initcap(ename) from emp;
--将字符串全部转换为大写
select upper(ename) from emp;
--将字符串全部转换为小写
select lower(ename) from emp;
--填充字符串
--lpad(字段名，字段的长度，不足的部分用这里从左边开始填充)
--rpad(字段名，字段的长度，不足的部分用这里从右边开始填充)
select lpad(ename, 10, '*') from emp;
select rpad(ename, 6, '/') from emp;
--去除空格
select trim(ename) from emp;--去除左右两边的空格
select ltrim(ename) from emp;--去除左空格
select rtrim(ename) from emp;--去除右空格
--从字符串中查找指定字符串的位置
select instr('ABDJIF', 'A') from emp;
select instr(ename, 'A') from emp;
--查看字符串的长度
select length(ename) from emp;
--截取字符串
select substr(ename, 0, 2) from emp;
--替换
select replace(ename,'ab', 'hehe') from emp;


2.数字函数：以NUMBER类型为参数返回NUMBER值

--给小数进行四舍五入操作，可以指定小数部分的位数
select round(123.123, 2) from dual;
select round(-44.32) from dual;
--截断数据，不会进行四舍五入
select trunc(123.129, 2) from dual;
--取模
select mod(10, 4) from dual;
select mod(-10, 6) from dual;
--向上取整
select ceil(12.56) from dual;
--向下取整
select floor(18.99) from dual;
--取绝对值
select abs(-1000) from dual;
--获取正负值:参数为正，返回1；参数为负数，返回-1；0返回0
select sign(100) from dual;
--取x的y次幂
select power(2, 3) from dual;


3.日期函数
--返回系统当前日期
select sysdate from dual;
--增加指定的月份
select add_mounths(hiredate, 2), hiredate from emp;
--返回本月的最后一天
select last_day(sysdate) from dual;
--两个日期相间隔的月份
select months_between(sysdate, hiredate) from emp;
--获取四舍五入后最近的第一天
select sysdate 当时日期,
round(sysdate) 最近0点日期,
round(sysdate, 'day') 最近星期日,
round(sysdate, 'month') 最近月初,
round(sysdate, 'q') 最近季初日期,
round(sysdate, 'year') 最近年初日期 from dual;
--返回下周某一天的日期,注意此处的星期是中文的，英文的不能识别，具体和软件版本有关
select next_day(sysdate, '星期一') from dual;
--提取日期中的时间
select
extract(hour from timestamp '2001-2-18 2:34:50') 小时,
extract(minute from timestamp '2001-2-18 2:34:50') 分钟,
extract(second from timestamp '2001-2-18 2:34:50') 秒,
extract(DAY from timestamp '2001-2-18 2:34:50') 日,
extract(MONTH from timestamp '2001-2-18 2:34:50') 月,
extract(YEAR from timestamp '2001-2-18 2:34:50') 年
from dual;
--返回日期的时间戳
select localtimestamp from dual;
select current_date from dual;
select current_timestamp from dual;
--给指定的时间单位增加数值
select 
trunc(sysdate) + (interval '1' second),  --加1秒
trunc(sysdate) + (interval '01:02:03' hour to second), --加指定小时、分钟和秒数
trunc(sysdate) + (interval '2 01:02' day to minute) --加指定天数、小时和分钟
from dual;


4.转化函数：to_char\to_number\to_date
--隐式转换：字符和数字的相互转换、字符和日期的相互转换
--显式转换：当由数值或者日期转成字符串的时候，必须要规定格式
select '999'+10 from dual;--结果是1009
--to_char:将日期或数据转换为char数据类型
select to_char(sysdate, 'YYYY-MM-dd') from dual;
select to_char(123.455632,'999999999') from dual; --‘99999999’相当于占位符
select to_char(123.2343432,'000.00') from dual;--‘000.00’也是占位符，但是会用0进行补位
select to_char(123.4342,'L0000.00') from dual;--L显示货币的符号
select to_char(12343434,'999,999,999,999') from dual; 
--to_number&to_date
select to_date('2019-09-07 10:10:10','YYYY/MM/dd HH24:MI:SS') from dual;
select to_number('4,445,235,234', '999,999,999') from dual;


5.其它函数
--条件函数:decode(条件,值1， 返回值1， 值2， 返回值2，。。。)
--条件函数:
	--case 条件 
	--when 值1 then 返回值1
	--when 值2 then 返回值2
	--when....
--给不同部门的人员涨薪,10部门涨10%，20部门涨20%，30部门涨30%
select ename,sal,deptno,
	decode(deptno,10,sal*1.1,20,sal*1.2,30,dal*1.3) 
	from emp;
select ename,dal,deptno,
	case deptno when 10 then sal*1.1 when 20 then sal*1.2 when 30 then sal*1.3
	from emp;
--表的行转列
select 
max(decode(type,1,value)) 姓名,
max(decode(type,2,value)) 性别,
max(decode(type,3,value)) 年龄
from test
group by t_id;
```



##### select 子句的顺序

from ->where条件过滤->group by按列分组，having分组前过滤->select 输出结果集->order by排序



### 关联查询



##### 92语法关联

在where子句中写入连接条件，连接条件可以是等值连接，也可以是非等值连接

连接的类型：等值连接、非等值连接、外连接、子连接

```sql
--等值连接：两个表中包含相同的列名
--查询将雇员和部门的名称
select ename,dname from emp, dept where emp.deptno = dept.deptno;

--非等值连接：两个表中没有相同的列名，但是可以通过值之间的非等值关系进行连接
--查询雇员名称以及自己的薪水等级
select e.ename,sg.grade from emp e,salgrade sg where e.sal between sg.losal and sg.hisal;

--外连接
--将雇员表中的数据和部门表中的数据都对应的进行显示,此时利用等值连接的话只会把关联到的数据显示出来，没有关联到的数据则不会显示，此时需要外连接
--分类：左外连接（以左表的数据为主）和右外连接（以右表的数据为主)
select * from emp e,dept d where e.deptno = d.deptno; --等值连接
select * from emp e,dept d where e.deptno = d.deptno(+);--左外连接
select * from emp e,dept d where d.deptno(+) = d.deptno;--右外连接

--自连接:将一张表当成不同的表，自己关联自己
--将雇员和他经理的名称查出来
select e.ename,m.ename from emp e, emp m where e.mgr = m.empno;

--笛卡尔积:关联多张表但是不指定连接条件的时候，会进行笛卡尔积
select * from emp e, dept d;



---92语法的问题
/*
在92语法中，多张表的连接条件会放到where子句中，同事，where需要对表进行条件过滤，因此，相当于将过滤条件和连接条件杂糅到一起，为了处理这个问题，产生了99语法
*/
```



##### 99语法

将联结条件和过滤条件分开，引入新的JOIN语法，使用on子句完成表连接，使用where进行条件过滤

```sql
--CROSS JOIN，相当于92语法中的笛卡尔积
select * from emp cross join dept;

--NATURE JOIN,相当于等值连接，但是注意不需要写连接条件，会从两张表中自动找到相同的列做连接
--当两张表中没有相同的列名的时候，会进行笛卡尔积操作,所以一般在使用时先确定两个表之间有相同字段
select * from emp e naturnal join dept d;
select * from emp e naturnal join salgrade sg;

--ON子句,添加连接条件,可以代替92语法中进行的的等值连接、非等值连接
select * from emp e join dept d on e.deptno = d.deptno;
select * from emp e join salgrade sg on e.sal between sg.losal and sg.hisal;

--OUTER JOIN
--LEFT OUTER JOIN 将左表中的全部数据正常显示，右表没有对应记录则直接显示空
select * from emp e left outer join dept d on e.deptno = d.deptno;
--RIGHT OUTER JOIN 
select * from emp e right outer join dept d on e.deptno = d.deptno;
--FULL OUTER JOIN 将左右表中的全部数据正常显示，表中若没有对应记录则直接显示空，相当于左外连接和右外连接的合集
select * from emp e full outer join dept d on e.deptno = d.deptno;

--INNER JOIN 等同于两张表的连接查询，只会查询出有匹配记录的数据
--INNER JOIN等同于JOIN等同于where，WHERE子句中使用的连接语句，在数据库语言中，被称为隐性连接。INNER JOIN……ON子句产生的连接称为显性连接。（其他JOIN参数也是显性连接）
select * from emp e inner join dept d on e.deptno = d.deptno;

--USING子句,除了可以使用on表示连接条件，也可以使用using作为连接条件,此时，连接条件的列不再归属于任何一张表
select * from emp e join dept d using(deptno);
select * from emp e join dept d on e.deptno = d.deptno;
select deptno from emp e join dept d using(deptno);(可以查询到deptno)
select e.deptno from emp e join dept d using(deptno);(不可以查询deptno，因为deptno不再属于emp或者dept，不能通过这两张表找到deptno)



--总结：两种语法的SQL语句没有任何限制，在公司中可以随意使用，但是建议使用99语法，不要使用92语法，SQL语句显得更清楚
```



```sql
--总结
inner join = join
left join 左连接
right join 右连接
full join 
```







### 子查询

子查询就是嵌套在其他查询中的查询，可以将子查询看做一张表。外层的语句可以把内嵌的子查询返回的结果当成一张表使用。

* 子查询要用括号括起来

* 将子查询放在比较运算符的右侧，增强可读性

* 分为单行子查询和多行子查询

  

```sql
--单行子查询,子查询返回一条记录，使用单行记录比较运算符
--1.有哪些人的薪水是在整个雇员的平均薪水之上的
	-- 先求平均薪水
	--把所有人的薪水和平均薪水作比较
select e.ename from e.emp where e.sal >= (select avg(e.sal) from emp e);


--多行子查询，子查询可以返回多条结果值,使用多行运算符，如in、some、all
--2.查询雇员中有哪些人是经理人
	--先查询所有的经理人编号
	select distinct e.mgr from emp e;
	--在雇员表中过滤这些编号
	select * from emp e where e.empno in (select e.mgr from emp e);
	

--在From子句中使用子查询
--3.每个部门平均薪水的等级
	--先求部门的平均薪水
	select e.deptno,avg(e.sal) from emp e group by e.deptno;
	--跟薪水等级表做关联，求出平均薪水的等级
	select t.deptno,sg.grade from salgrade sg join (select e.deptno,avg(e.sal) vsal from emp e group by e.deptno) t on t.vsal between sg.losal and sg.hisal;

```



```sql
--1.求平均薪水最高的部门的部门编号
--求部门的平均薪水
select e.deptno,avg(e.sal) from emp e group by e.deptno
--求平均薪水最高的部门
select max(t.vsal) from (select e.deptno,avg(e.sal) vsal from emp e group by e.deptno) t
--求部门编号
select t.deptno 
from (select e.deptno,avg(e.sal) vsal from emp e group by e.deptno) t
where t.vsal = 
(select max(t.vsal) from (select e.deptno,avg(e.sal) vsal from emp e group by e.deptno) t);

--2.求部门平均的薪水等级
--求部门每个人的薪水等级
select e.deptno,sg.grade from emp e join salgrade sg on e.sal between sg.losal and sg.hisal
--按照部门分组求平均薪水等级
select t.deptno,avg(t.grade) from 
(select e.deptno,sg.grade from emp e join salgrade sg on e.sal between sg.losal and sg.hisal t) 
group by t.deptno;

--限制输出：limit，是mysql中用来做限制输出的，但是Oracle中不能这么使用
--在oracle中进行限制输出和分页的功能时，必须要使用rownum，但是rownum不能直接使用，rownum是在虚拟表中创建的字段，只有在嵌套查询时才可以使用到rownum
--3.求薪水最高的前5名雇员
select * 
from (select * from emp e order by e.sal desc) t1
where rownum <=5;

--4.求薪水最高的第6到10名雇员
select * from (select * from emp e order by e.sal desc) t1 
where rownum>5 and rownum <=10;
--上面的语句是错误的，因为rownum的值其实是在随着过滤条件动态生成的，如果先进行>5的过滤，那么rownum就会跳过前5条记录，将原来的第6条作为rownum=1开始生成，所以可以将某个范围先作为过滤条件过滤，同时保存此时的rownum作为一个字段，然后再对新生成的这张表通过rownum进行过滤
select * 
from (select t1.*,rownum rn from (select * from emp e order by e.sal desc) t1 where rownum <=10) t where t.rn > 5 and t.rn <=10;
--或
select * 
from (select t1.*, rownum from (select * from emp e order by e.sal desc) t1) t
where t.rn > 5 and t.rn <= 10;
```





### Oracle表设计



##### 视图view

* 也称为虚表，不占用物理空间，因为视图本身的定义语句还是要存储在数据字典里的，视图只有逻辑定义。每次使用的时候只是重新执行SQL。
* 视图是从一个或多个实际表中获得的，这些表的数据存放在数据库中。那些用于产生视图的表叫做该视图的基表。一个视图也可以从另一个视图中产生。
* 视图的定义存在数据库中，于此定义相关的数据并没有再存一份于数据库中，通过视图看到的数据存放在基表中。
* 视图看上去非常像数据库的物理表，对它的操作同任何其它的表一样。当通过视图修改数据时，实际上是在改变基表中的数据；相反地，基表数据的改变也会自动反映在由基表产生的视图中。由于逻辑上的原因，有些Oracle视图可以修改对应的基表，有些则不能（仅仅能查询）

* Oracle中特有的**物化视图**：占用物理空间，它的更新是通过设置决定的，一共有两种设置：on commit和on demand来确定是和基表同时更新还是在使用时更新
* 在查询时，只需从视图中查询，无需再写完全的select查询语句
* 当视图不再被需要的时候，使用“drop view”删除视图，不会对数据造成影响，因为视图只是基于数据库的表之上的一个查询定义。

* 创建视图的语法

```sql
CREATE [OR REPLACE]  --创建或者替换
VIEW view     -- 视图名称
[(alias[, alias]...)] --别名
AS subquery  --子查询
[WITH READ ONLY]; --只读视图，无法修改
```



* 授权视图：普通用户在第一次使用视图时如果提示没有权限，需要管理员先为用户授权

```sql
--使用system用户为scott用户增加权限
grant create view, create table to scott;
--使用system用户为scott解锁
alter user scott account unlock;
--回收权限
revoke create view from scott;
```



* 视图的使用

```sql
--管理员为用户授权创建视图
grant create view to scott;
--创建视图
create view v_emp as select * from emp deptno = 30;
--视图的使用
select * from v_emp;
--向视图中添加数据,执行成功之后需要提交事务，因为这条数据在执行后是被写入某个缓存区域，只在当前会话有效，提交事务后会将这条数据写入文件，在数据库中真实生效
insert into v_emp(empno, ename) values(1111, 'test');
--如果定义的视图是非只读视图可以插入数据，只读视图则不可以插入数据
create view v_emp2 as(select * from emp) with read only;
insert into v_emp2(empno, ename) values(123, 'dfi');--无法插入，只读视图只能查询，不能增删改
--删除视图
drop view v_emp2;
--当删除视图中的数据的时候，如果数据来源于多个基表，则此时不能全部进行删除，只能删除一个表中的数据
```



* 视图练习

```sql
--求平均薪水的等级最低的部门(完全使用子查询)
--通过视图可以将重复的sql语句给抽象出来
--1.求平均薪水
select e.deptno,avg(sal) from emp e group by e.deptno;
--2.求平均薪水的等级
select t.deptno, sg.grade gd from salgrade sg join (select e.deptno,avg(sal) vsal from emp e group by e.deptno) t on t.vsal between sg.losal and sg.hisal;
--3.求平均薪水的等级最低的部门
select min(t.gd) from salgrade sg join (select t.deptno, sg.grade from salgrade sg join (select e.deptno,avg(sal) vsal from emp e group by e.deptno) t on t.vsal between sg.losal and sg.hisal) t
--4.求平均薪水的等级最低的部门的部门名称
select d.dname, d.deptno from dept d join (select t.deptno, sg.grade gd from salgrade sg join (select e.deptno, avg(e.sal) vsal from emp e group by e.deptno) t on t.vsal between sg.losal and sg.hisal) t on t.deptno = d.deptno where t.gd = (select min(t.gd) from salgrade sg join (select t.deptno, sg.grade from salgrade sg join (select e.deptno,avg(sal) vsal from emp e group by e.deptno) t on t.vsal between sg.losal and sg.hisal) t)

--求平均薪水的等级最低的部门(使用视图)
--创建视图
create view v_deptno_grade as select t.deptno, sg.grade gd from salgrade sg join (select e.deptno, avg(e.sal) vsal from emp e group by e.deptno) t on t.vsal between sg.losal and sg.hisal;
--使用视图替换后
SELECT
	d.dname,d.deptno 
FROM dept d
JOIN v_deptno_grade t ON t.deptno = d.deptno 
WHERE t.gd = 
   (SELECT min( t.gd ) 
	FROM v_deptno_grade t);

```



##### 用户管理



```sql
-- 创建用户
create user [username] identified by [password];
-- 查看用户是否创建
select username from dba_users;
--用户授权
grant
privileges -- 授予的权限
[on object_name] --某个库的某个表
to username --给某个用户

grant create session to John;
--收回授予用户John的scott用户表emp的所有权限
revoke all on scott.emp from John;
--查看自己的权限
select * from user_sys_privs;
```



##### 序列sequence

* 是Oracle专有的对象，用来产生一个自动递增的数列，mysql中可以直接对字段进行值自动递增的设置。
* 语法



```sql
create sequence seq_name   --设置序列名字seq_name
increment by n				--每次增长的值是n
start with n				--从n开始
maxvalue |no maxvalue 10^27 or -1	--最大值
minvalue |no minvalue		--最小值
cycle| nocycle				--是否循环
cache|nocache				--是否设定缓存，将序列中会被使用的值预先存在缓存中，提高效率
```



```sql
--在Oracle中如果需要完成一个列的自增操作，必须要使用序列
--创建序列
create sequence my_sequence
increment by 2
start with 1

--如何使用序列
--注意，如果创建好序列之后，没有经过任何的使用，那么不能获取当前的值，必须要先执行nextval之后才能获取当前值
--1.查看当前序列的值
select my_sequence.currval from dual;
--2.获取序列的下一个值
select my_sequence.nextval from dual;
--插入值的时候使用序列
insert into emp(empno, ename) values(my_sequence.nextval, 'hehe');
--删除序列
drop sequence seq_name;
```





### DML 数据库操作语言



* 在实际项目中，使用最多的是读取操作，但是插入数据和删除数据同等重要，而修改操作相对较少

* 增删改在进行操作的时候都需要”事务“的保证，也就是说每次在pl/sql中执行sql语句之后都需要完成commit操作，所以事务变得非常关键。

* 事务存在的最主要的目的是为了保证数据一致性，如果同一份数据在同一个时刻只能有一个人访问就不会出现数据错乱的问题，但是在现在的项目中，更多的是并发访问，并发访问同时带来的就是数据的不安全，也就是不一致。

* 如果要保证数据的安全，最主要的方式就是加锁的方式，MVCC

* 事务的延伸：

  * 最基本的数据库事务
  * 声明式事务
  * 分布式事务

* 为了提高效率，有可能多个操作会在同一个事务中执行，那么就有可能部分成功，部分失败，基于这样的情况，就需要事务的控制。

  select * from emp where id = 908 for update //排它锁

  select * from emp where id = 897 lock in share mode //共享锁

* 如果不保证事务的话，会造成脏读，不可重复读，幻读



##### 增——插入操作

* 元组值的插入

* 查询结果的插入



```sql
--元组值的插入：最基本的插入方式

--1.如果表名之后没有列，那么只能将所有的列都插入
insert into tablename values(val1,val2,...)
--2.可以指定向哪些列中插入数据
--向部分列插入数据的时候，不是想向那个列插入就插入的，要遵循创建表的时候定义的规范
--需要满足两个条件：该列定义为允许null值；在表定义中给出默认值，当不给定值时，可以使用默认值
insert into tablename(col1,col2,...) values(val1, val2,...)
```



```sql
--查询结果的插入

--创建表的其它方式
--复制另一张表的数据和结构生成新表, 不会复制约束
create table emp2 as select * from emp;
--只能复制表结构，不会复制约束
create table emp3 as select * from emp where 1=2;
--如果有一个集合的数据，把集合中的所有数据都挨条插入，效率一定极低，所以一般在实际的操作中，很少一条条插入，更多的是批量插入

```



##### 删



* sql的删除操作是指从基本表中删除元组

```sql
--删除满足条件的数据
delete from emp2 where deptno = 15;
--删除所有数据，需要提交事务
delete from emp2;
--删除全部数据,truncate不同于delete，delete在进行删除时经过事务，truncate不经过事务，效率较高，但是一旦删除就是永久删除，不具备回滚的能力，风险较高，慎重使用
truncate table emp2;
```



##### 改



```sql
--UPDATE tablename set col2 = val1, col2 = val2,... where condition
--可以更新或者修改满足条件的一个列或多个列
--更新单列的值
update emp set ename = 'hiee' where ename = 'hehe';
--更新多列的值
update emp set job = 'teacher', mgr=7902 where empno = 15;
```



### 事务（Transaction）



* 事务是一个操作序列，这些操作要么都做要么都不做，是一个不可分割的工作单位，是数据库环境中的逻辑工作单位。

* 事务是为了保证数据库的完整性

* 事务不能嵌套
* 在Oracle中，没有事务开始的语句，一个Transaction起始于一条DML（Insert、Update、Delete）语句，结束于一下的几种情况：
  * 用户显式执行Commit语句提交操作或Rollback语句回退。
  * 当执行DDL（Create、ALter、Drop）语句事务自动提交
  * 用户正常断开连接时，Transaction自动提交
  * 系统崩溃或断电时事务自动回退



```sql
-- 事务的开始是一条DML语句
insert into emp(empno, ename) values(1111, 'zhangsan');
--事务的结束：
--1.正常的commit（使数据修改生效）或者rollback（将数据恢复到上一个状态）
commit;
rollback;
--2.自动提交，但是一般情况下要将自动提交进行关闭，因为自动提交会在每一条语句执行后进行提交，效率太低
--3.用户关闭会话之后，会自动提交事务
--4.系统崩溃或者断电时会回滚事务，也就是将数据恢复到上一个状态。
```



##### Commit & Rollback



* commit表示事务成功地结束，此时告诉系统，数据库要进入一个新的正确状态，该事务对数据库的所有更新都已交付实施。每个commit语句都可以看成是一个事务成功地结束，同时也是另一个事务的开始。

* Rollback表示事务不成功的结束，此时告诉系统，已发生错误，数据库可能出在不正确的状态，该事务对数据库的更新必须被撤销，数据库应恢复该事务到初始状态。每个Rollback语句同时也是另一个事务的开始。

* 一旦执行了commit语句，将目前对数据库的操作提交给数据库（实际写入DB），以后就不能用Rollback进行撤销。

* 执行一个DDL，DCL语句或从SQL*PLUS正常退出，都会自动执行commit命令。

* savepoint 保存点：当一个操作集合中包含多条SQL语句，但只想让其中某部分成功，某部分失败，此时可以使用保存点。需要回滚到某一个状态的话可以使用Rollback to 保存点。

  ```sql
  delete from emp where empno = 1111;
  delete from emp where empno = 2222;
  savepoint sp1;
  delete from emp where empno = 1234;
  rollback to sp1;-- 将会回滚到保存点的状态
  commit;-- 此时只是删除了1111和2222
  ```

  

##### 事务的四个特性：ACID

* 原子性（Atomicity）：表示不可分割，一个操作集合要么全部成功，要么全部失败，不可以从中间做切分。
* 一致性（consistency）：为了保证数据的一致性，当经过N多个操作之后，数据的状态不会改变。从一个一致性状态到另一个一致性状态，也就是数据不能发生错乱
* 隔离性（Isolation）：各个事务之间不会产生影响（隔离级别）。严格的隔离性会导致效率降低，在某些情况下为了提高程序的执行效率，需要降低隔离的级别。
  * 隔离级别：读未提交--读已提交--可重复读--序列化
  * 由于隔离级别设置的问题会导致数据不一致的问题：脏读、不可重复读、幻读
* 持久性：所有数据的修改必须要持久化到存储介质中，不会因为应用程序的关闭而导致数据的丢失。

四种特性中，哪个是最关键的？

	* 所有的特性中都是为了保证数据的一致性，所以一致性是最终的追求。
	* 事务中的一致性是通过原子性、隔离性、持久性来保证的











