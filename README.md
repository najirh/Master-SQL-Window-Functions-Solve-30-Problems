# Window Function & Problems & Datasets

1. **Five Datasets** to use for different exercises.
2. **Ten Problems** for each window function: 5 for basic to medium, and 5 for advanced, including usage of `PARTITION BY` and `ORDER BY`.

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
5. **Find the row number of each employee within the department.**
   - Use `ROW_NUMBER() OVER (PARTITION BY department ORDER BY hire_date)`.

#### Advanced:
6. **Find the employee with the highest salary in each department.**
   - Use `RANK()` and `WHERE rank = 1`.
7. **Calculate the rank difference between departments for employees with the same salary.**
   - Combine `RANK()` with a self-join.
8. **Get the 2nd highest salary in each department.**
   - Use `RANK()` and `WHERE rank = 2`.
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
(5, 'Tablet', '2023-05-15', 600.00, 'Chicago');
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
   - Use `LAG()` with a `WHERE` clause.
8. **For each sale, show the amount from two sales before.**
   - Use `LAG(amount, 2) OVER (ORDER BY sale_date)`.
9. **Compare current sales with the next two sales for each product.**
   - Use `LEAD(amount, 2) OVER (PARTITION BY product_name ORDER BY sale_date)`.
10. **Find the running total of sales for each store location.**
    - Use `SUM(amount) OVER (PARTITION BY store_location ORDER BY sale_date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)`.

---

### 3. **Dataset 3: Student Scores**

**Schema**:
```sql
CREATE TABLE student_scores (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(50),
    subject VARCHAR(50),
    score INT,
    exam_date DATE
);

INSERT INTO student_scores (student_id, student_name, subject, score, exam_date)
VALUES
(1, 'John', 'Math', 85, '2023-05-15'),
(2, 'Jane', 'Math', 95, '2023-05-15'),
(3, 'Jake', 'History', 78, '2023-06-10'),
(4, 'Julia', 'History', 88, '2023-06-10'),
(5, 'Jill', 'Science', 92, '2023-07-01');
```

### Problems: ROW_NUMBER, RANK, DENSE_RANK, LEAD, LAG
#### Basic to Medium:
1. **Rank students by their score in each subject.**
   - Use `RANK() OVER (PARTITION BY subject ORDER BY score DESC)`.
2. **Find the row number of students based on their exam date.**
   - Use `ROW_NUMBER() OVER (ORDER BY exam_date)`.
3. **Identify the score difference between consecutive exams for each student.**
   - Use `LAG(score) OVER (PARTITION BY student_name ORDER BY exam_date)`.
4. **Show the next student’s score for each subject.**
   - Use `LEAD(score) OVER (PARTITION BY subject ORDER BY exam_date)`.
5. **Calculate the dense rank of students based on their score across all subjects.**
   - Use `DENSE_RANK() OVER (ORDER BY score DESC)`.

#### Advanced:
6. **Rank students within each subject, but reset the rank after every 5 students.**
   - Use `NTILE(5) OVER (PARTITION BY subject ORDER BY score DESC)`.
7. **Find students who improved their score compared to the previous exam.**
   - Use `LAG()` and a `WHERE` clause.
8. **Calculate the cumulative score for each student in all subjects.**
   - Use `SUM(score) OVER (PARTITION BY student_name ORDER BY exam_date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)`.
9. **Find the second-highest score in each subject.**
   - Use `RANK()` with `WHERE`.
10. **Show students who have not improved compared to their previous exam score.**
    - Use `LAG(score)` with comparison.

---

Would you like more datasets or customization on any problem sets?
