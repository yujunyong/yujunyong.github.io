title: mysql常用sql
date: 2015-09-29 00:40:23
categories: database
tags: mysql
---

# 查询
## 在where子句中引用取别名的列
``` sql
select * from
(
  select sal as salary, comm as commission
  from emp
) as x
where salary < 5000;
```

## 在select语句中使用条件逻辑
``` sql
select ename, sal,
	case when sal <= 2000 then 'UNDERPAID'
		 when sal >= 4000 then 'OVERPAID'
		 else 'OK'
	end as status
from emp;
```

## 从表中随机返回n条记录
``` sql
select ename, job from emp order by rand() limit 5;
```

## 找到满足这样条件的员工：即他的收入比紧随其后聘用的员工要少
``` sql
select ename, sal, hiredate from
(
	select a.ename, a.sal, a.hiredate,
		(select min(hiredate) from emp b
			where b.hiredate > a.hiredate
			and b.sal > a.sal) as next_sal_grtr,
		(select min(hiredate) from emp b
			where b.hiredate > a.hiredate) as next_hire
	from emp a
) x
where next_sal_grtr = next_hire;
```

## 返回一个结果集，它包含每个部门中所有员工的姓名，所在部门，工资，聘用日期以及部门中最新聘用员工的工资
``` sql
select e.deptno, e.ename, e.sal, e.hiredate,
	(select max(d.sal) from emp d
		where d.deptno = e.deptno and d.hiredate =
			(select max(f.hiredate) from emp f
				where f.deptno = e.deptno)) as latest_sal
from emp e
order by e.deptno, e.hiredate desc;
```

<!-- more -->

## 查找空值
``` sql
select * from emp where comm is null;
```


## 将空值转换为实际值
``` sql
select coalesce(comm, 0) from emp;
```

``` sql
select case
	when comm is null then 0
	else comm
	end as comm
from emp;
```
## 在运算和比较时使用null值
null值永远不会等于或不等于任何值，也包括null值自己。在计算的数据有包含null值需要进行转换。
``` sql
select ename, comm
from emp
where coalesce(comm, 0) < (select comm from emp where ename = 'WARD');
```

# 排序
## 按子串排序
``` sql
select ename, job from emp order by substr(job, length(job) - 2);
```

## 根据条件排序
``` sql
select ename, sal, job, comm from emp
order by case when job = 'SALESMAN' then comm else sal end;
```

# 多表
## 在一个表中查找与其他表不匹配的记录
``` sql
select d.* from dept d
left outer join emp e on (d.deptno = e.deptno)
where e.deptno is null;
```

## 聚集与联接
查询部门10中所有员工的工资合计和奖金合计

``` sql
select deptno, sum(distinct sal) as total_sal, sum(bonus) as total_bonus from
(
select e.empno,
	e.ename,
	e.sal,
	e.deptno,
	e.sal*case when eb.type = 1 then .1
		when eb.type = 2 then .2
		else .3
		end as bonus
from emp e left outer join emp_bonus eb
on (e.empno = eb.empno)
and e.deptno = 10
) x
group by deptno;
```
emp_bonus表中的数据可能存在，一些员工有多条数据，一些员工没有数据的情况。所以求sal总和时`distinct`不能少，因为在子查询中sal可能有重复数据，这里还假定emp表中sal没有重复的数据；必需使用`left outer join`进行关联查询，这样才能保证关联查询时sal的数据不会丢失。

查询部门10和部门20所有员工的部门信息，并返回部门30和40（但不包含员工信息）的部门信息
``` sql
select e.ename, d.deptno, d.dname, d.loc
from dept d left join emp e
on (d.deptno = e.deptno
	and (e.deptno=10 or e.deptno=20))
order by d.deptno;
```

# 插入更新删除
## 复制表定义
``` sql
create table dept_east like dept;
```
## 从一个表向另一个表复制行
``` sql
insert into dept_east (deptno, dname, loc)
select deptno, dname, loc
from dept
where loc in ('NEW YORK', 'BOSTON');
```
## 删除重复数据
``` sql
delete n1 from names n1, names n2 where n1.id > n2.id and n1.name = n2.name
```

``` sql
delete n1 from names n1, names n2 where n1.id < n2.id and n1.name = n2.name
```
``` sql
delete from names where not in (select min(id) from names group by name)
```

# 附录
names表
``` plain
+----+--------+
| id | name   |
+----+--------+
| 1  | google |
| 2  | yahoo  |
| 3  | msn    |
| 4  | google |
| 5  | google |
| 6  | yahoo  |
+----+--------+
```

emp表
``` plain
+-------+--------+-----------+------+------------+------+------+--------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL  | COMM | DEPTNO |
+-------+--------+-----------+------+------------+------+------+--------+
|  7369 | SMITH  | CLERK     | 7902 | 1980-12-17 |  800 | NULL |     20 |
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600 |  300 |     30 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250 |  500 |     30 |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975 | NULL |     20 |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250 | 1400 |     30 |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850 | NULL |     30 |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450 | NULL |     10 |
|  7788 | SCOTT  | ANALYST   | 7566 | 1982-12-09 | 3000 | NULL |     20 |
|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000 | NULL |     10 |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500 |    0 |     30 |
|  7876 | ADAMS  | CLERK     | 7788 | 1983-01-12 | 1100 | NULL |     20 |
|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950 | NULL |     30 |
|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000 | NULL |     20 |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300 | NULL |     10 |
+-------+--------+-----------+------+------------+------+------+--------+
```
dept表
``` plain
+--------+------------+----------+
| DEPTNO | DNAME      | LOC      |
+--------+------------+----------+
|     10 | ACCOUNTING | NEW YORK |
|     20 | RESEARCH   | DALLAS   |
|     30 | SALES      | CHICAGO  |
|     40 | OPERATIONS | BOSTON   |
+--------+------------+----------+
```
emp_bonus表
``` plain
+-------+------------+------+
| empno | received   | type |
+-------+------------+------+
|  7934 | 2005-05-17 |    1 |
|  7934 | 2005-02-15 |    2 |
|  7839 | 2005-02-15 |    3 |
|  7782 | 2005-02-15 |    1 |
+-------+------------+------+
```
