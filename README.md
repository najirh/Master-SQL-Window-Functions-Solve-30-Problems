### 30 SQL QUESTIONS ON WINDOW FUNCTIONS: BASICS TO ADVANCED LEVEL

- **Key Concepts**: Understand how window functions differ from standard aggregates.
- **Real-World Datasets**: Work with practical datasets focusing on `ROW_NUMBER`, `RANK`, `LEAD`, and `LAG`.
- **Hands-On Problems**: Solve curated challenges from basic to advanced levels.
- **Step-by-Step Solutions**: Follow clear explanations for each problem.

Want to dive deeper? Join our **Live SQL A-Z Workshop**! Over **3 days and 15 hours**, learn SQL comprehensively and complete two live projects.

[**Join Our Live SQL Workshop!**](https://zeroanalyst.com/course/)

---

### 1. **Dataset 1: Employee Salaries**

**Schema**:
```sql
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(50),
    department VARCHAR(50),
    salary DECIMAL(10, 2),
    hire_date DATE
);

INSERT INTO employees (emp_id, emp_name, department, salary, hire_date)
VALUES
(1, 'Alice', 'HR', 55000, '2018-01-15'),
(2, 'Bob', 'IT', 75000, '2017-05-23'),
(3, 'Charlie', 'Finance', 82000, '2019-03-12'),
(4, 'Diana', 'IT', 60000, '2020-07-19'),
(5, 'Eve', 'HR', 52000, '2021-11-05'),
(6, 'Frank', 'Finance', 72000, '2020-08-10'),
(7, 'Grace', 'HR', 61000, '2016-12-20'),
(8, 'Hank', 'IT', 69000, '2019-01-11'),
(9, 'Ivy', 'Finance', 73000, '2018-09-30'),
(10, 'Jack', 'HR', 54000, '2017-10-15'),
(11, 'Kate', 'IT', 78000, '2016-06-01'),
(12, 'Leo', 'HR', 59000, '2019-02-21'),
(13, 'Mia', 'Finance', 76000, '2019-04-10'),
(14, 'Nick', 'IT', 65000, '2018-12-05'),
(15, 'Olivia', 'HR', 53000, '2020-09-29'),
(16, 'Paul', 'Finance', 70000, '2021-03-22'),
(17, 'Quincy', 'IT', 72000, '2020-01-07'),
(18, 'Rita', 'HR', 60000, '2020-05-15'),
(19, 'Steve', 'Finance', 78000, '2019-08-18'),
(20, 'Tom', 'IT', 81000, '2018-07-23'),
(21, 'Uma', 'HR', 58000, '2020-02-17'),
(22, 'Victor', 'Finance', 75000, '2021-05-10'),
(23, 'Wendy', 'IT', 70000, '2020-10-05'),
(24, 'Xander', 'HR', 62000, '2017-11-22'),
(25, 'Yara', 'Finance', 82000, '2021-06-30');
```

### Problems: ROW_NUMBER, RANK, DENSE_RANK
#### Basic to Medium:
1. **Find the row number of each employee ordered by salary.**
   - Use `ROW_NUMBER() OVER (ORDER BY salary DESC)`.
2. **Rank employees based on their salaries.**
   - Use `RANK() OVER (ORDER BY salary DESC)`.
3. **Dense rank employees within each department based on salary.**
   - Use `DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC)`.
4. **Rank employees by hire date.**
   - Use `RANK() OVER (ORDER BY hire_date ASC)`.
5. **Find the row number of each employee within their department based on hire date.**
   - Use `ROW_NUMBER() OVER (PARTITION BY department ORDER BY hire_date)`.

#### Advanced:
6. **Show the employee name and salary of the highest-paid employee in each department.**
   - Use `RANK()` and filter for `WHERE rank = 1`.
7. **Calculate the rank difference between employees with the same salary across departments.**
   - Combine `RANK()` with a self-join.
8. **Get the 2nd highest salary in each department, showing the employee name.**
   - Use `RANK()` and filter for `WHERE rank = 2`.
9. **Find the average salary of the top 3 highest-paid employees in each department.**
   - Use `ROW_NUMBER()` with a `PARTITION BY department`.
10. **Assign a dense rank to employees based on their hire date, resetting at each department change.**
    - Combine `DENSE_RANK()` with `PARTITION BY department ORDER BY hire_date`.

---

### 2. **Dataset 2: Sales Data**

**Schema**:
```sql
CREATE TABLE sales (
    sale_id INT PRIMARY KEY,
    product_name VARCHAR(50),
    sale_date DATE,
    amount DECIMAL(10, 2),
    store_location VARCHAR(50)
);

INSERT INTO sales (sale_id, product_name, sale_date, amount, store_location)
VALUES
(1, 'Laptop', '2023-01-15', 1500.00, 'New York'),
(2, 'Headphones', '2023-02-03', 200.00, 'Chicago'),
(3, 'Laptop', '2023-03-10', 1600.00, 'San Francisco'),
(4, 'Phone', '2023-02-21', 800.00, 'New York'),
(5, 'Tablet', '2023-05-15', 600.00, 'Chicago'),
(6, 'Laptop', '2023-04-12', 1450.00, 'Chicago'),
(7, 'Phone', '2023-01-20', 850.00, 'San Francisco'),
(8, 'Headphones', '2023-03-18', 220.00, 'New York'),
(9, 'Tablet', '2023-04-25', 620.00, 'San Francisco'),
(10, 'Laptop', '2023-06-10', 1550.00, 'New York'),
(11, 'Phone', '2023-07-02', 790.00, 'Chicago'),
(12, 'Tablet', '2023-07-20', 640.00, 'New York'),
(13, 'Headphones', '2023-05-30', 210.00, 'San Francisco'),
(14, 'Laptop', '2023-08-15', 1620.00, 'Chicago'),
(15, 'Phone', '2023-09-01', 800.00, 'San Francisco'),
(16, 'Tablet', '2023-08-23', 650.00, 'New York'),
(17, 'Headphones', '2023-10-05', 230.00, 'Chicago'),
(18, 'Laptop', '2023-11-12', 1580.00, 'New York'),
(19, 'Phone', '2023-12-10', 815.00, 'Chicago'),
(20, 'Tablet', '2023-12-28', 680.00, 'San Francisco'),
(21, 'Laptop', '2023-11-22', 1650.00, 'San Francisco'),
(22, 'Headphones', '2023-11-30', 240.00, 'New York'),
(23, 'Phone', '2023-12-15', 820.00, 'New York'),
(24, 'Tablet', '2023-12-05', 670.00, 'Chicago'),
(25, 'Laptop', '2023-12-28', 1700.00, 'Chicago');
```

### Problems: LEAD, LAG
#### Basic to Medium:
1. **Calculate the difference in sales amount between consecutive sales.**
   - Use `LAG(amount, 1) OVER (ORDER BY sale_date)`.
2. **Find the sale amount for the next product sale.**
   - Use `LEAD(amount, 1) OVER (ORDER BY sale_date)`.
3. **For each product, calculate the difference between the current and previous sale amount.**
   - Use `LAG(amount) OVER (PARTITION BY product_name ORDER BY sale_date)`.
4. **Show the next product sold for each store location.**
   - Use `LEAD(product_name) OVER (PARTITION BY store_location ORDER BY sale_date)`.
5. **Find the previous sale amount for each store location.**
   - Use `LAG(amount) OVER (PARTITION BY store_location ORDER BY sale_date)`.

#### Advanced:
6. **Calculate the rolling difference between sales for each product.**
   - Use both `LEAD` and `LAG` for comparison.
7. **Identify sales where the previous sale amount was higher than the current sale.**
   - Use `L

AG()` and a conditional filter.
8. **Show total sales for each month, with the amount sold in the previous month.**
   - Use `SUM(amount) OVER (PARTITION BY MONTH(sale_date))` combined with `LAG`.
9. **Determine the percentage increase or decrease in sales compared to the previous sale.**
   - Use `(amount - LAG(amount) OVER (ORDER BY sale_date)) / LAG(amount)`.
10. **List all sales where the amount is greater than the average sales amount for that product.**
    - Combine window functions with a subquery to find the average.

---

### 3. **Dataset 3: Student Scores**

**Schema**:
```sql
CREATE TABLE student_scores (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(50),
    exam_date DATE,
    score INT,
    class VARCHAR(50)
);

INSERT INTO student_scores (student_id, student_name, exam_date, score, class)
VALUES
(1, 'Alice', '2023-01-15', 85, 'A'),
(2, 'Bob', '2023-01-15', 90, 'A'),
(3, 'Charlie', '2023-01-15', 78, 'A'),
(4, 'Diana', '2023-02-01', 88, 'B'),
(5, 'Eve', '2023-02-01', 95, 'B'),
(6, 'Frank', '2023-02-01', 84, 'B'),
(7, 'Grace', '2023-03-10', 92, 'A'),
(8, 'Hank', '2023-03-10', 87, 'A'),
(9, 'Ivy', '2023-03-10', 82, 'A'),
(10, 'Jack', '2023-04-15', 76, 'B'),
(11, 'Kate', '2023-04-15', 89, 'B'),
(12, 'Leo', '2023-04-15', 91, 'B'),
(13, 'Mia', '2023-05-12', 80, 'A'),
(14, 'Nick', '2023-05-12', 93, 'A'),
(15, 'Olivia', '2023-05-12', 87, 'A'),
(16, 'Paul', '2023-06-05', 86, 'B'),
(17, 'Quincy', '2023-06-05', 88, 'B'),
(18, 'Rita', '2023-06-05', 90, 'B'),
(19, 'Steve', '2023-07-07', 95, 'A'),
(20, 'Tom', '2023-07-07', 89, 'A'),
(21, 'Uma', '2023-07-07', 84, 'A'),
(22, 'Victor', '2023-08-10', 78, 'B'),
(23, 'Wendy', '2023-08-10', 90, 'B'),
(24, 'Xander', '2023-08-10', 85, 'B'),
(25, 'Yara', '2023-09-12', 92, 'A');
```

### Problems: RANK, DENSE_RANK
#### Basic to Medium:
1. **Rank students based on their scores for each exam.**
   - Use `RANK() OVER (PARTITION BY exam_date ORDER BY score DESC)`.
2. **Determine the dense rank of students based on their scores.**
   - Use `DENSE_RANK() OVER (ORDER BY score DESC)`.
3. **Calculate the score difference between the current and previous exam for each student.**
   - Use `LAG(score) OVER (PARTITION BY student_name ORDER BY exam_date)`.
4. **List students who improved their score compared to the previous exam.**
   - Use `LAG(score)`, and filter results where the difference is greater than 0.
5. **Determine the highest score for each class.**
   - Use `MAX(score) OVER (PARTITION BY class)`.

#### Advanced:
6. **Reset the rank for students after every 5 students based on their scores.**
   - Use a combination of `ROW_NUMBER()` and `DENSE_RANK()`.
7. **Show the count of students who improved their score compared to the previous exam.**
   - Use a conditional count with `LAG()`.
8. **Find the percentage increase in scores for each student compared to their previous exam.**
   - Use `(score - LAG(score) OVER (PARTITION BY student_name ORDER BY exam_date)) / LAG(score)`.
9. **List all students with their scores and whether they improved from the last exam (yes/no).**
   - Use a `CASE` statement combined with `LAG()`.
10. **Rank students within their class, considering ties.**
    - Use `RANK()` and partition by class.
