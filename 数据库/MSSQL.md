## 目录

- [一. 试用SQL查询语句表达下列对教学数据库中三个基本表 S、SC 、C 的查询](#一-试用sql查询语句表达下列对教学数据库中三个基本表-ssc-c-的查询)
  - [1．求年龄大于所有女同学年龄的男学生姓名和年龄。](#1求年龄大于所有女同学年龄的男学生姓名和年龄)



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

