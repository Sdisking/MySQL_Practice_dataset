# MySQL_Practice_dataset

## Creating Database

```sql
Create database Practice_file;
use Practice_file;
Create table practice (
employee_id int primary key,
first_name	varchar(20),
last_name	varchar(20),
department	varchar(20),
location	varchar(20),
salary	int,
hire_date	Date,
performance_score int);
```

```sql
select * from practice;
```

## Exploratory Analysis

**how many employees we have**

```sql
select Count(employee_id) as Total_Employees from practice;
```

**how many Departments are there?**

```sql
select distinct department from practice;
```

**Average salary of employees**

```sql
select avg(salary) as Avg_salary from practice;
```


## Basic

**1)List all employees from the IT department.**

```sql
select * from practice where department="IT";
```

**2)Show employees who work in Mumbai with salary above ₹1,00,000.**

```sql
select * from practice where location ="mumbai" and salary>100000;
```

**3)Count how many employees are in each department.**

```sql
select department,count(employee_id) as No_of_employees from practice group by department;
```

**4)Find the highest salary in the dataset.**

```sql
select max(salary) from practice;
```


**5) Get all employees hired after 2020-01-01.**

```sql
select * from practice where Hire_date > "2020-01-01";
```


## Intermediate

**6. Show the average salary by department.**

```sql
select department ,avg(salary) as Avg_salary from practice group by department;
```

**7. List the top 5 highest-paid employees.**

```sql
select * from practice order by salary desc limit 5;
```

**8. Find employees with a performance_score > 8.**

```sql
select * from practice where performance_score >8;
```

**9. Show how many employees work in each location.**

```sql
select location ,count(employee_id) as total_employees from practice group by location;
```

**10. Find the earliest and latest hire_date.**

```sql
select max(hire_date) as latest_Hire_date,min(hire_date) as earliest_Hire_date from practice;
```


## Advance

**11. Find the second-highest salary in each department.**

```sql
select * from 
(select department,salary,rank() over(partition by department order by salary desc) as ranked from
(select department, salary from practice) as abc) as xyz where ranked=2;
```


**11 b. Find the highest salary in each department.**

```sql
select department,salary from
(select department,salary,rank() over(partition by department order by salary desc) as ranked from
(select department,salary from practice) as abc) as xyz where ranked =1 ;
```


**12. Rank employees in Finance by salary using window functions.**

```sql
select employee_id,first_name,last_name,rank() over(order by salary desc) as ranked from
(select * from practice where department="finance") as abc;
```


**13. Show the cumulative average salary over time by hire_date.**

```sql
select hire_date,avg(avg_salary) over(order by hire_date) as cummulative_avg_salary from
(select hire_date,avg(salary) as avg_salary from practice group by hire_date) as abc;
```


**14. Find employees whose salary is above the department average.**

```sql
select employee_id,First_name,practice.department,practice.salary,abc.avg_salary 
from practice 
join (select department,avg(salary) as Avg_salary from practice 
group by department) as abc 
on abc.department=practice.department 
where practice.salary > avg_salary 
order by department,salary desc;
```


**15. Create a view that shows employee name, department, salary, and whether they are Above/Below Avg Department Salary.**

```sql
Create view salary_Rating as
select first_name,department,salary,case when salary>(select avg(salary) from practice) then "ABOVE" else "Below" end as Salary_status from practice;

select * from salary_rating;
```

## End-----
