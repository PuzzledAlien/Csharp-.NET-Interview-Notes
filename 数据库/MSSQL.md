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
- [二、试用 SQL 更新语句表达对教学数据库中三个基本表 S、SC 、C的各个更新操作](#二试用-sql-更新语句表达对教学数据库中三个基本表-ssc-c的各个更新操作)
  - [1 ．在基本表 SC 中修改 4 号课程的成绩，若成绩小于等于 75 分时提高 5% ， 若成绩大于 75 分时提高 4% （用两个 UPDATE 语句实现）。](#1-在基本表-sc-中修改-4-号课程的成绩若成绩小于等于-75-分时提高-5--若成绩大于-75-分时提高-4-用两个-update-语句实现)
  - [2 ．把低于总平均成绩的女同学成绩提高 5% 。](#2-把低于总平均成绩的女同学成绩提高-5-)
  - [3 ．把选修数据库原理课不及格的成绩全改为空值。](#3-把选修数据库原理课不及格的成绩全改为空值)
  - [4 ．把WANG 同学的学习选课和成绩全部删去。](#4-把wang-同学的学习选课和成绩全部删去)
  - [5 ．在基本表 SC 中删除尚无成绩的选课元组。](#5-在基本表-sc-中删除尚无成绩的选课元组)
  - [6 ．在基本表 S 中检索每一门课程成绩都大于等于 80 分的学生学号、姓名和性别， 并把检索到的值送往另一个已存在的基本表 S1 （ Sno ， SNAME ， SSEX ）。](#6-在基本表-s-中检索每一门课程成绩都大于等于-80-分的学生学号姓名和性别-并把检索到的值送往另一个已存在的基本表-s1--sno--sname--ssex-)
  - [7．往基本表 S 中插入一个学生元组（ ‘ S9’，‘ WU ’，18 ）。](#7往基本表-s-中插入一个学生元组--s9-wu-18-)



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

### 2 ．把低于总平均成绩的女同学成绩提高 5% 。

```mssql
UPDATE SC
SET GRADE=GRADE*1.05
WHERE GRADE<(SELECT AVG(GRADE) FROM SC)
AND Sno IN (SELECT Sno FROM S WHERE SSEX=' 女')
```

### 3 ．把选修数据库原理课不及格的成绩全改为空值。

```mssql
UPDATE SC SET GRADE=NULL
WHERE GRADE<60 AND Cno IN ( SELECT Cno FROM C
WHERE CNAME='数据库原理' )
```

### 4 ．把WANG 同学的学习选课和成绩全部删去。

```mssql
DELETE FROM SC WHERE Sno IN(SELECT Sno FROM S
WHERE SNAME='WANG')
```

### 5 ．在基本表 SC 中删除尚无成绩的选课元组。

```mssql
DELETE FROM SC WHERE GRADE IS NULL
```

### 6 ．在基本表 S 中检索每一门课程成绩都大于等于 80 分的学生学号、姓名和性别， 并把检索到的值送往另一个已存在的基本表 S1 （ Sno ， SNAME ， SSEX ）。

```mssql
SELECT Sno,SNAME,SSEX INTO s1 FROM student DELETE FROM s1
INSERT INTO S1(Sno,SNAME,SSEX) SELECT Sno,SNAME,SSEX
FROM S WHERE NOT EXISTS(SELECT * FROM SC WHERE GRADE<80 AND S.Sno=SC.Sno)
SELECT * FROM s1
```

考虑：以上会有什么问题？

```mssql
INSERT INTO S1(Sno,SNAME,SSEX) SELECT Sno,SNAME,SSEX
FROM S WHERE NOT EXISTS(SELECT * FROM SC WHERE
GRADE<80 AND S.Sno=SC.Sno OR S.Sno=SC.Sno AND gradeis null) AND sno IN (SELECT sno FROM sc)
```

### 7．往基本表 S 中插入一个学生元组（ ‘ S9’，‘ WU ’，18 ）。

```mssql
INSERT INTO S(Sno,SNAME,SAGE) VALUES('59','WU',18)
```

## 三、问题描述：为管理岗位业务培训信息，建立 3 个表

S (Sno,SN,SD,SA) Sno,SN,SD,SA 分别代表学号、学员姓名、所属单位、学员年龄 

C (Cno,CN ) Cno,CN 分别代表课程编号、课程名称

SC ( Sno,Cno,G ) Sno,Cno,G 分别代表学号、所选修的课程编号、学习成绩

要求实现如下 5 个处理：

### 1.使用标准 SQL 嵌套语句查询选修课程名称为’税收基础’的学员学号和姓名

```mssql
SELECT SN,SD FROM S WHERE [Sno] IN(
SELECT [Sno] FROM C,SC
WHERE C.[Cno]=SC.[Cno] AND CN=N'税收基础')
```

### 2.使用标准 SQL 嵌套语句查询选修课程编号为’C2’的学员姓名和所属单位

```mssql
SELECT S.SN,S.SD FROM S,SC WHERE S.[Sno]=SC.[Sno]
AND SC.[Cno]='C2'
```

### 3.使用标准 SQL 嵌套语句查询不选修课程编号为’C5’的学员姓名和所属单位

```mssql
SELECT SN,SD FROM S WHERE [Sno] NOT IN(
SELECT [Sno] FROM SC
WHERE [Cno]='C5')
```

### 4.使用标准 SQL 嵌套语句查询只选修了一门课程的学员姓名和所属单位

```mssql
SELECT SN,SD FROM S WHERE [Sno] IN(
SELECT [Sno] FROM SC INNER JOIN C ON SC.[Cno]=C.[Cno] GROUP BY [Sno]
HAVING COUNT(*)=1)
```

### 5.查询选修了课程的学员人数

```mssql
SELECT 学员人数 = COUNT(DISTINCT[Sno]) FROM SC
```

### 6.查询选修课程超过 5 门的学员学号和所属单位

```mssql
SELECT SN,SD FROM S WHERE [Sno] IN(
SELECT [Sno] FROM SC GROUP BY [Sno]
HAVING COUNT(DISTINCT [Cno])>5)
```

## 四、问题描述：已知关系模式

S(SNO,SNAME ） 学生关系。 SNO 为学号， SNAME 为姓名

C (CNO,CNAME,TEACHER) 课程关系。 CNO 为课程号， CNAME 为课程名，

TEACHER 为任课教师

SC(SNO,CNO,GRADE) 选课关系。 GRADE 为成绩



要求实现如下 5 个处理：

### 1.找出没有选修过“李明”老师讲授课程的所有学生姓名

```mssql
SELECT sname FROM s WHERE NOT EXISTS (SELECT * FROM c,sc WHERE c.cno=sc.cno AND c.teacher=N'李明' AND s.sno=sc.sno)
```

或

```mssql
SELECT sno,sname FROM s WHERE sno NOT IN
(SELECT sno FROM sc,c WHERE c.cno=sc.cno AND c.teacher=N'liu')
```

### 2 ．列出有二门以上（含两门）不及格课程的学生姓名及其平均成绩

```mssql
SELECT S.SNO,SNAME,AVG_GRADE=AVG(SC.GRADE) FROM S,SC
WHERE GRADE<60 AND s.sno=sc.sno GROUP BY s.SNO,s.sname
HAVING COUNT(DISTINCT CNO)>=2
```

或

```mssql
SELECT S.SNO,S.SNAME,AVG_GRADE=AVG(SC.GRADE) FROM S,SC,(
SELECT SNO FROM SC
WHERE GRADE<60
GROUP BY SNO
HAVING COUNT(DISTINCT CNO)>=2
)A WHERE S.SNO=A.SNO AND SC.SNO=A.SNO GROUP BY S.SNO,S.SNAME
```

### 3 ． 列出既学过“ 1 ”号课程，又学过“ 2 ”号课程的所有学生姓名

```mssql
SELECT S.SNO,S.SNAME
FROM S,sc
WHERE S.SNO=SC.SNO AND cno='1' AND s.sno IN (SELECT S.sno FROM S,sc
WHERE S.SNO=SC.SNO AND cno='2')
```

或

 ```mssql
SELECT S.SNO,S.SNAME FROM S,(
SELECT SC.SNO FROM SC,C
WHERE SC.CNO=C.CNO
AND C.cno IN('1','2') GROUP BY SNO
HAVING COUNT(DISTINCT c.CNO)=2
)SC WHERE S.SNO=SC.SNO
 ```

### 4 ．列出“ 1 ”号课成绩比“ 04010002 ”号同学该门课成绩高的所有学生的学号

```mssql
SELECT S.SNO,S.SNAME FROM S,SC
WHERE SC.CNO='1'and SC.sNO=S.sNO
AND grade>(SELECT grade FROM s,sc
WHERE s.SNO='04010002' AND SC.CNO='1' AND SC.sNO=S.sNO)
```

### 5 ． 列出“ 1 ”号课成绩比“ 2 ”号课成绩高的所有学生的学号及其“ 1 ”号课和“ 2 ”号课的成绩

```mssql
SELECT SC1.SNO,[1 号课成绩 ]=SC1.GRADE,[2 号课成绩 ]=SC2.GRADE FROM SC SC1,SC SC2
WHERE SC1.CNO='1' AND SC2.CNO='2'
AND SC1.SNO=SC2.SNO
AND SC1.GRADE>SC2.GRADE
```

## 四、通配符理解题（请说出下列通配符的含义）

### 1．LIKE 'Mc%' 

将搜索以字母 Mc 开头的所有字符串（如 McBadden）。

### 2．LIKE '%inger' 

将搜索以字母 inger 结尾的所有字符串（如 Ringer 、Stringer ）。

### 3．LIKE '%en%' 

将搜索在任何位置包含字母 en 的所有字符串（如 Bennet 、Green、McBadden）。

 

### 4．LIKE '_heryl' 

将搜索以字母 heryl 结尾的所有六个字母的名称（如Cheryl、Sheryl ）。

 

### 5．LIKE '[M-Z]inger' 

将搜索以字符串 inger 结尾、以从 M 到 Z 的任何单个字母开头的所有名称（如 Ringer ）。

 

### 6．`LIKE 'M[^c]%' `

将搜索以字母 M 开头，并且第二个字母不是 c 的所有名称（如 MacFeather ）。

 

 

## 五．问答题

### 1.请自定义分页存储过程！