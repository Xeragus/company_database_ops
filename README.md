# MySQL-Ops

**Which department pays the highest salary currently and who is the department manager?**
```sql
SELECT departments.dept_name                                  AS Department, 
       Concat(employees.first_name, ' ', employees.last_name) AS Manager 
FROM   departments 
       JOIN dept_manager 
         ON departments.dept_no = dept_manager.dept_no 
            AND dept_manager.to_date >= CURRENT_DATE 
       JOIN employees 
         ON dept_manager.emp_no = employees.emp_no 
       JOIN dept_emp 
         ON departments.dept_no = dept_emp.dept_no 
       JOIN salaries 
         ON dept_emp.emp_no = salaries.emp_no 
            AND salaries.to_date >= CURRENT_DATE 
            AND salaries.emp_no = (SELECT salaries.emp_no 
                                   FROM   salaries 
                                   WHERE  salaries.to_date >= CURRENT_DATE 
                                   ORDER  BY salaries.salary DESC 
                                   LIMIT  1); 
```
**How many employees are working in Human Resources Department currently?**
```sql
SELECT Count(DISTINCT employees.emp_no) AS 'Number of employees', 
       departments.dept_name            AS Department 
FROM   employees 
       JOIN dept_emp 
         ON employees.emp_no = dept_emp.emp_no 
            AND dept_emp.to_date >= CURRENT_DATE 
       JOIN departments 
         ON dept_emp.dept_no = departments.dept_no 
            AND departments.dept_name = 'Human Resources'; 
```
**Who is currently the oldest employee of the company?**
```sql
SELECT first_name       AS 'First name', 
       last_name        AS 'Last name', 
       birth_date       AS 'Birth date', 
       employees.emp_no AS 'Employee number' 
FROM   dept_emp 
       JOIN employees 
         ON dept_emp.emp_no = employees.emp_no 
            AND dept_emp.to_date >= CURRENT_DATE 
ORDER  BY birth_date, 
          employees.emp_no 
LIMIT  1;  
```
**What is the current average salary of Marketing Department?**
```sql
SELECT Avg(salaries.salary)  AS 'Average salary', 
       departments.dept_name AS Department 
FROM   salaries 
       JOIN dept_emp 
         ON dept_emp.emp_no = salaries.emp_no 
            AND dept_emp.to_date >= CURRENT_DATE 
       JOIN departments 
         ON departments.dept_no = dept_emp.dept_no 
            AND departments.dept_name = 'Marketing'; 
```
**Who is currently the newest employee of the company?**
```sql
SELECT first_name AS 'First name', 
       last_name  AS 'Last name', 
       hire_date  AS 'Hire date' 
FROM   dept_emp 
       JOIN employees 
         ON dept_emp.emp_no = employees.emp_no 
            AND dept_emp.to_date >= CURRENT_DATE 
ORDER  BY hire_date DESC, 
          employees.emp_no 
LIMIT  1; 
```
