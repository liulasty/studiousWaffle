Oracle数据库中的常用SQL语句涵盖了数据定义语言（DDL）、数据操纵语言（DML）、数据查询语言（DQL）等多个方面。以下是一些常用的Oracle SQL语句及其简要说明：

### 数据定义语言（DDL）

1. **创建表（CREATE TABLE）**
    
    ```sql
    CREATE TABLE 表名 (      列名1 数据类型 [约束],      列名2 数据类型 [约束],      ...  );
    ```
    
    例如，创建一个学生表：
    
    ```sql
    CREATE TABLE Student (      id VARCHAR2(32) PRIMARY KEY,      name VARCHAR2(8) NOT NULL,      age NUMBER  );
    ```
    
2. **创建索引（CREATE INDEX）**
    
    ```sql
    CREATE [UNIQUE] INDEX 索引名 ON 表名(列名);
    ```
    
    例如，为学生表的id和name列创建唯一索引：
    
    ```sql
    CREATE UNIQUE INDEX stu_index ON Student(id, name);
    ```
    
3. **修改表结构（ALTER TABLE）**
    
    - 添加新字段：
        
        ```sql
        ALTER TABLE 表名 ADD (字段名 数据类型 [约束]);
        ```
        
    - 修改现有字段：
        
        ```sql
        ALTER TABLE 表名 MODIFY (字段名 数据类型 [约束]);
        ```
        
    - 删除现有字段：
        
        ```sql
        ALTER TABLE 表名 DROP COLUMN 字段名;
        ```
        
    - 重命名字段：
        
        ```sql
        ALTER TABLE 表名 RENAME COLUMN 列名 TO 新列名;
        ```
        
4. **删除表（DROP TABLE）**
    
    ```sql
    DROP TABLE 表名;
    ```
    
5. **添加/删除注释**
    
    - 表注释：
        
        ```sql
        COMMENT ON TABLE 表名 IS '表的注释信息';
        ```
        
    - 字段注释：
        
        ```sql
        COMMENT ON COLUMN 表名.字段名 IS '字段的注释信息';
        ```
        

### 数据操纵语言（DML）

1. **查询（SELECT）**
    
    ```sql
    SELECT 列名 FROM 表名 WHERE 条件;
    ```
    
    例如，查询学生表中所有学生的姓名和年龄：
    
    ```sql
    SELECT name, age FROM Student;
    ```
    
2. **插入（INSERT）**
    
    ```sql
    INSERT INTO 表名 (列1, 列2, ...) VALUES (值1, 值2, ...);
    ```
    
    例如，向学生表中插入一条新记录：
    
    ```sql
    INSERT INTO Student (id, name, age) VALUES ('001', '张三', 20);
    ```
    
3. **更新（UPDATE）**
    
    ```sql
    UPDATE 表名 SET 列名 = 新值 WHERE 条件;
    ```
    
    例如，将学生表中id为'001'的学生的年龄更新为21岁：
    
    ```sql
    UPDATE Student SET age = 21 WHERE id = '001';
    ```
    
4. **删除（DELETE）**
    
    ```sql
    DELETE FROM 表名 WHERE 条件;
    ```
    
    例如，删除学生表中id为'001'的学生记录：
    
    ```sql
    DELETE FROM Student WHERE id = '001';
    ```
    

### 数据查询语言（DQL）中的高级用法

1. **GROUP BY**  
    用于将结果集按照一个或多个列进行分组，常与聚合函数（如COUNT, MAX, MIN, SUM, AVG）一起使用。
    
2. **DISTINCT**  
    用于返回唯一不同的值。
    
3. **JOIN**  
    用于根据两个或多个表中的列之间的关系，从这些表中查询数据。
    
4. **ORDER BY**  
    用于对结果集进行排序。
    
5. **子查询**  
    在SELECT、INSERT、UPDATE、DELETE语句中嵌套另一个SELECT语句。
    

### 注意事项

- 在执行DDL语句（如CREATE TABLE, DROP TABLE等）时，需要谨慎操作，因为这些操作会直接影响数据库的结构。
- 在执行DML语句（如INSERT, UPDATE, DELETE等）时，建议先进行备份或确保有恢复机制，以防数据丢失或错误操作。
- Oracle SQL语句的具体语法可能会随着Oracle数据库版本的更新而有所变化，因此建议参考最新的官方文档。