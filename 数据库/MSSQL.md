## 目录

- [一. 试用SQL查询语句表达下列对教学数据库中三个基本表 S、SC 、C 的查询](#一-试用sql查询语句表达下列对教学数据库中三个基本表-ssc-c-的查询)
  - [1．求年龄大于所有女同学年龄的男学生姓名和年龄。](#1求年龄大于所有女同学年龄的男学生姓名和年龄)
  - [2.求年龄大于女同学平均年龄的男学生姓名和年龄。](#2求年龄大于女同学平均年龄的男学生姓名和年龄)
  - [3.在 SC 中检索成绩为空值的学生学号和课程号。](#3在-sc-中检索成绩为空值的学生学号和课程号)
  - [4.检索姓名以 WANG 打头的所有学生的姓名和年龄。](#4检索姓名以-wang-打头的所有学生的姓名和年龄)
  - [5.检索学号比 WANG 同学大，而年龄比他小的学生姓名。](#5检索学号比-wang-同学大而年龄比他小的学生姓名)
  - [6.统计每门课程的学生选修人数 （超过 2 人的课程才统计） 。要求输出课程号和选修人数，查询结果按人数降序排列，若人数相同，按课程号升序排列。](#6统计每门课程的学生选修人数-超过-2-人的课程才统计-要求输出课程号和选修人数查询结果按人数降序排列若人数相同按课程号升序排列)
  - [7.求 LIU 老师所授课程的每门课程的学生平均成绩。](#7求-liu-老师所授课程的每门课程的学生平均成绩)
  - [8.求选修 C4 课程的学生的平均年龄。](#8求选修-c4-课程的学生的平均年龄)
  - [9.统计有学生选修的课程门数。](#9统计有学生选修的课程门数)



## 一. 试用SQL查询语句表达下列对教学数据库中三个基本表 S、SC 、C 的查询

S(sno,sname,SAGE,SSEX) 各字段表示学号，姓名，年龄，性别

Sc(sno,cno,grade) 各字段表示学号，课程号，成绩、

C(cno,cname, TEACHER) 各字段表示课程号，课程名和教师名 其 中 SAGE， grade 是数值型，其他均为字符型。

 

要求用 SQL 查询语句实现如下处理：

### 1．求年龄大于所有女同学年龄的男学生姓名和年龄。

```mssql
SELECT SNAME,SAGE FROM S AS X
WHERE X.SSEX=' 男'AND X.SAGE >ALL (SELECT SAGE FROM S AS Y WHERE
Y.SSEX=' 女')
```

### 2.求年龄大于女同学平均年龄的男学生姓名和年龄。

```mssql
SELECT SNAME,SAGE
FROM S
WHERE SSEX=' 男'
AND SAGE>(SELECT AVG(SAGE) FROM S WHERE SSEX='女')
```

### 3.在 SC 中检索成绩为空值的学生学号和课程号。

```mssql
SELECT Sno,Cno FROM SC WHERE GRADE IS NULL
```

### 4.检索姓名以 WANG 打头的所有学生的姓名和年龄。

```mssql
SELECT SNAME,SAGE FROM S
WHERE SNAME LIKE 'WANG%'
```

### 5.检索学号比 WANG 同学大，而年龄比他小的学生姓名。

```mssql
SELECT X.SNAME FROM S AS X, S AS Y
WHERE Y.SNAME='WANG' AND X.Sno>Y.Sno AND X.SAGE<Y.SAGE
```

或

```mssql
SELECT SNAME
FROM s
WHERE sno>(SELECT sno FROM s WHERE SNAME='WANG') 
AND SAGE<(SELECT sAGE FROM s WHERE SNAME='WANG')
```

### 6.统计每门课程的学生选修人数 （超过 2 人的课程才统计） 。要求输出课程号和选修人数，查询结果按人数降序排列，若人数相同，按课程号升序排列。

```mssql
SELECT DISTINCT Cno,COUNT(Sno) 
FROM SC
GROUP BY Cno 
HAVING COUNT(Sno)>2
ORDER BY 2 DESC, Cno ASC
```

或

```mssql
SELECT DISTINCT Cno,COUNT(Sno) AS 人数
FROM SC 
GROUP BY Cno
HAVING COUNT(Sno)>2
ORDER BY 人数 DESC, Cno ASC
```

### 7.求 LIU 老师所授课程的每门课程的学生平均成绩。

```mssql
SELECT AVG(GRADE)
FROM SC JOIN C ON SC.Cno=C.Cno WHERE TEACHER='liu'
GROUP BY c.Cno
```

或

```mssql
SELECT CNAME,AVG(GRADE) FROM SC,C 
WHERE SC.Cno=C.Cno AND TEACHER='liu'
GROUP BY c.Cno,cname
```

### 8.求选修 C4 课程的学生的平均年龄。

```mssql
SELECT AVG(SAGE)
FROM S WHERE Sno
IN(SELECT Sno FROM SC WHERE Cno='4')
```

或

```mssql
SELECT AVG(SAGE)
FROM S,SC WHERE S.Sno=SC.Sno AND Cno='4'
```

### 9.统计有学生选修的课程门数。

```mssql
SELECT COUNT(DISTINCT Cno) FROM SC
```

## 二、试用 SQL 更新语句表达对教学数据库中三个基本表 S、SC 、C的各个更新操作

要求用 SQL 更新语句实现如下处理：

### 1 ．在基本表 SC 中修改 4 号课程的成绩，若成绩小于等于 75 分时提高 5% ， 若成绩大于 75 分时提高 4% （用两个 UPDATE 语句实现）。

```mssql
UPDATE SC SET GRADE=GRADE*1.05 WHERE Cno='4' AND GRADE<=75;
UPDATE SC SET GRADE=GRADE*1.04 WHERE Cno='4' AND GRADE>75;
```

