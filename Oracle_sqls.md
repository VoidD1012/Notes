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



```sql

```









