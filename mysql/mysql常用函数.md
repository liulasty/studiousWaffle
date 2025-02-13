MySQL 提供了许多常用函数，可以帮助你进行数据处理和分析。以下是一些常见的 MySQL 函数分类及示例：

### 1. 字符串函数
- **CONCAT ()**: 连接两个或多个字符串。
  ```sql
  SELECT CONCAT('Hello', ' ', 'World'); -- 输出: Hello World
  ```

- **UPPER ()**: 将字符串转换为大写。
  ```sql
  SELECT UPPER('hello'); -- 输出: HELLO
  ```

- **LOWER ()**: 将字符串转换为小写。
  ```sql
  SELECT LOWER('HELLO'); -- 输出: hello
  ```

- **SUBSTRING ()**: 返回字符串的一部分。
  ```sql
  SELECT SUBSTRING('Hello World', 1, 5); -- 输出: Hello
  ```

- **LENGTH ()**: 返回字符串的长度。
  ```sql
  SELECT LENGTH('Hello'); -- 输出: 5
  ```

### 2. 数学函数
- **ABS ()**: 返回绝对值。
  ```sql
  SELECT ABS(-5); -- 输出: 5
  ```

- **ROUND ()**: 四舍五入。
  ```sql
  SELECT ROUND(123.4567, 2); -- 输出: 123.46
  ```

- **RAND ()**: 返回 0 到 1 之间的随机浮点数。
  ```sql
  SELECT RAND(); 
  ```

- **FLOOR ()**: 返回小于或等于指定数的最大整数。
  ```sql
  SELECT FLOOR(3.7); -- 输出: 3
  ```

### 3. 日期和时间函数
- **NOW ()**: 返回当前日期和时间。
  ```sql
  SELECT NOW(); 
  ```

- **CURDATE ()**: 返回当前日期。
  ```sql
  SELECT CURDATE(); 
  ```

- **DATEDIFF ()**: 计算两个日期之间的天数。
  ```sql
  SELECT DATEDIFF('2023-10-01', '2023-09-01'); -- 输出: 30
  ```

- **DATE_ADD ()**: 在日期上添加时间间隔。
  ```sql
  SELECT DATE_ADD('2023-10-01', INTERVAL 1 DAY); -- 输出: 2023-10-02
  ```

### 4. 聚合函数
- **COUNT ()**: 计算行数。
  ```sql
  SELECT COUNT(*) FROM table_name; 
  ```

- **SUM ()**: 计算总和。
  ```sql
  SELECT SUM(column_name) FROM table_name; 
  ```

- **AVG ()**: 计算平均值。
  ```sql
  SELECT AVG(column_name) FROM table_name; 
  ```

- **MAX ()**: 返回最大值。
  ```sql
  SELECT MAX(column_name) FROM table_name; 
  ```

- **MIN ()**: 返回最小值。
  ```sql
  SELECT MIN(column_name) FROM table_name; 
  ```

### 5. 控制流函数
- **IF ()**: 条件判断。
  ```sql
  SELECT IF(condition, true_value, false_value); 
  ```

- **CASE**: 多条件判断。
  ```sql
  SELECT CASE WHEN condition1 THEN result1
              WHEN condition2 THEN result2
              ELSE result3
         END;
  ```
