# TOPS_DA_SQL_
Data Analytics
 Assingment1;

-- 1. Display all information in the tables EMP and DEPT.
     select * from hrdb;
     
-- 2. Display only the hire date and employee name for each employee.
		
	select HireDate, EMP_Name from hrdb;
    
-- 3. Display the ename concatenated with the job ID, separated by a comma and space, and 
-- name the column Employee and Title

    select concat_ws(", ",EMP_Name,EMP_id ) EmployeeAndTitle from hrdb;


-- 4. Display the hire date, name and department number for all clerks.

        select Hiredate,EMP_Name,DEP_id from hrdb where Job_name = 'clerk';

-- 5. Create a query to display all the data from the EMP table. Separate each column by a 
 --   comma. Name the column THE OUTPUT
	 select  concat_ws(",",EMP_id,EMP_Name,Job_name,HireDate,DEP_id) THE_OUTPUT from hrdb; 
        
-- 6. Display the names and salaries of all employees with a salary greater than 2000.

	    select EMP_Name, EMP_Salary from hrdb where EMP_Salary  >2000;
        
--  7. Display the names and dates of employees with the column headers "Name" and "Start 
 -- Date" 
		select EMP_Name,HireDate as Name_StartDate from hrdb ;
        
-- 8. Display the names and hire dates of all employees in the order they were hired.
       
       select EMP_Name,HireDate  from hrdb order by HireDate asc ;
       
-- 9. Display the names and salaries of all employees in reverse salary order.

	select EMP_Name,EMP_Salary from hrdb order by EMP_Salary desc;

-- 10. Display 'ename" and "deptno" who are all earned commission and display salary in 
--   reverse order. 

		select EMP_Name,DEP_id,commission,EMP_Salary from hrdb order by EMP_Salary desc;

-- 11. Display the last name and job title of all employees who do not have a manager

	select last_name, job_name from hrdb where not job_Name =  'manager';
    
--  12. Display the last name, job, and salary for all employees whose job is sales representative 
--   or stock clerk and whose salary is not equal to $2,500, $3,500, or $5,000
    select last_name,Job_name,EMP_Salary from hrdb
    where Job_name in('salesman','clerk') and EMP_Salary not in (2500,3500,5000);

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
-- Assingment 2


SELECT @@GLOBAL.sql_mode = (select replace(@@sql_mode,'ONLY_FULL_GROUP_BY','ONLY_FULL_GROUP_BY'));
SET sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',""));

/*1) Display the maximum, minimum and average salary and commission earned.*/

select max(salary),min(salary),avg(salary),avg(commission_pct) from employees;

/*2) Display the department number, total salary payout and total commission payout for 
each department.*/

select department_id, sum(salary),sum(ifnull(commission_pct,'no commission'))  from employees   
 group by department_id  ;
 
 /*3) Display the department number and number of employees in each department.*/

select department_id, count(employee_id) CountEmp from employees group by department_id;

/*4) Display the department number and total salary of employees in each dep*/

select department_id,sum(salary) from employees group by department_id;

/*5) Display the employee's name who doesn't earn a commission. Order the result set 
without using the column name*/

select first_name from employees where commission_pct is null  order by 1;

/*6) Display the employees name, department id and commission. If an Employee doesn't 
earn the commission, then display as 'No commission'. Name the columns appropriatel*/

select first_name, department_id, ifnull(commission_pct,"No commission") EmpCommisssion
 from employees ;

/*7) Display the employee's name, salary and commission multiplied by 2. If an Employee 
doesn't earn the commission, then display as 'No commission. Name the columns 
appropriately*/

select first_name, salary*2,ifnull(commission_pct,"No commission")*2 from employees ;

/*8) Display the employee's name, department id who have the first name same as another 
employee in the same department*/

select first_name, department_id from employees
group by department_id having count(distinct first_name)>1 ;

/*9) Display the sum of salaries of the employees working under each Manager.*/

select sum(salary),first_name from employees group by manager_name;

/*10) Select the Managers name, the count of employees working under and the department 
ID of the manager.*/

select manager_name, count(employee_id),department_id from employees group by manager_name;

/*11) Select the employee name, department id, and the salary. Group the result with the 
manager name and the employee last name should have second letter 'a!*/
 select first_name,department_id,salary,concat_ws('/',manager_name,last_name) as "mangName_/Emp_lastName"
 from employees where manager_name like '_a%' and 
 last_name like '_a%';

 select first_name,department_id,salary
 from employees where manager_name like '_a%' and 
 last_name like '_a%' group by manager_name,last_name;

/*12) Display the average of sum of the salaries and group the result with the department id. 
Order the result with the department id.*/

select avg(salary) from employees group by department_id order by department_id;

/*13) Select the maximum salary of each department along with the department id*/

select max(salary), department_id from employees;

/*14) Display the commission, if not null display 10% of salary, if null display a default value 1*/

SELECT 
    commission_pct,
    CASE 
	  WHEN commission_pct IS NOT NULL THEN 0.1 * salary
	ELSE 1
END AS calculated_commission
FROM employees;

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Assingment 3
/*1. Write a query that displays the employee's last names only from the string's 2-5th 
position with the first letter capitalized and all other letters lowercase, Give each column an 
appropriate label.*/

       select concat(upper(substring(last_name,2,1)), lower(substring(last_name,3,4))) as Modified from employees;

/*2. Write a query that displays the employee's first name and last name along with a " in 
between for e.g.: first name : Ram; last name : Kumar then Ram-Kumar. Also displays the 
month on which the employee has joined.*/

	select first_name, last_name,monthname(hire_date) from employees where first_name like '%a_' and last_name like '%a_';

/*3. Write a query to display the employee's last name and if half of the salary is greater than 
ten thousand then increase the salary by 10% else by 11.5% along with the bonus amount of 
1500 each. Provide each column an appropriate label.*/

				select last_name,
				 case 
				 when salary * 0.5 > 10000 then round(salary * 1.1 + 1500, 2)
				 else round(salary * 1.115 + 1500)
				 end as IncreasedSalary
				from employees;

/*4. Display the employee ID by Appending two zeros after 2nd digit and 'E' in the end, 
department id, salary and the manager name all in Upper case, if the Manager name 
consists of 'z' replace it with '$!*/

SELECT
    CONCAT(SUBSTRING(employee_id, 1, 2), '00', 'E') AS modified_employee_id,
    department_id,
    UPPER(salary) AS upper_case_salary,
    UPPER(REPLACE(manager_name, 'z', '$!')) AS modified_manager_name
FROM
    employees;


/*5. Write a query that displays the employee's last names with the first letter capitalized and 
all other letters lowercase, and the length of the names, for all employees whose name 
starts with J, A, or M. Give each column an appropriate label. Sort the results by the 
employees' last names*/

SELECT
    CONCAT(UPPER(SUBSTRING(last_name, 1, 1)), LOWER(SUBSTRING(last_name, 2))) AS capitalized_last_name,
    LENGTH(last_name) AS name_length
FROM
    employees
WHERE
    SUBSTRING(last_name, 1, 1) IN ('J', 'A', 'M')
ORDER BY
    last_name;

/*6. Create a query to display the last name and salary for all employees. Format the salary to 
be 15 characters long, left-padded with $. Label the column SALARY*/

SELECT
    last_name,
    LPAD(CONVERT(salary, CHAR), 15, '$') AS SALARY
FROM
   emplopyees;

/*7. Display the employee's name if it is a palindrome*/
	select first_name from employees where first_name = reverse(first_name);

/*8. Display First names of all employees with initcaps.*/
-- initcaps funtion is basically not inbuild in the mysql Workbanch,
-- but with the use of upper/lower and substring funtions we perform the initcaps.

select concat(ucase(substring(first_name,1 ,1 )), lcase(substring(first_name,2))) as initcaps_name from employees;

/*9. From LOCATIONS table, extract the word between first and second space from the 
STREET ADDRESS column.*/

select  street_address,
 substring_index(substring_index(street_address, ' ',2), ' ', -1) as ExtractMidname from locations; 

/*10. Extract first letter from First Name column and append it with the Last Name. Also add 
"@systechusa.com" at the end. Name the column as e-ma  */

select concat(substring(first_name,1,1) ,last_name,'@systechusa.com') as email from employees;

/*11. Display the names and job titles of all employees with the same job as Trenna.*/
-- i have job_id in place of job_title
		select e.first_name,e.job_id,d.department_id from employees e
        join departments d on d.department_id = e.department_id where e.job_id = (select job_id from employees where first_name = 'Trenna');
SELECT
  e.first_name,
  e.last_name,
  j.job_title FROM
  employees e
JOIN
  jobs j ON e.job_id = j.job_id
WHERE
  e.job_id = (SELECT job_id FROM employees WHERE first_name = 'Trenna');
/* 12. Display the names and department name of all employees working in the same city as 
Trenna.*/

/*13. Display the name of the employee whose salary is the lowest.*/

SELECT first_name,last_name,salary from employees where salary = (select min(salary) from employees);

/*14. Display the names of all employees except the lowest paid*/

select first_name from employees where salary > (select min(salary) from employees);
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
-- Assingment 4
-- 1. Write a query to display the last name, department number, department name for all 
-- employees.

select e.last_name,d.department_id,d.department_name from employees e
 join  departments d on d.department_id = e.department_id ;
 
/*2. Create a unique list of all jobs that are in department 4. Include the location of the 
department in the output.*/

select   distinct e.job_id,d.department_id,d.location_id 
from  departments d join employees e on d.department_id = e.department_id
 group by e.job_id,d.department_id,d.location_id having d.department_id = '30'; 


/*3. Write a query to display the employee last name,department name,location id and city of 
all employees who earn commission.*/

	select e.last_name as 'Last_name t1' ,e.commission_pct 'commission t1',
    d.department_name 'department_name t2',d.location_id 'location t2',
    l.city 'City t3' 
    from employees e join departments d on e.department_id = d.department_id 
    join locations l on d.location_id = l.location_id  where e.commission_pct is not null;
    

/*4. Display the employee last name and department name of all employees who have an 'a' 
in their last name*/

select e.last_name, d.department_name from employees e join
 departments d on d.department_id = e.department_id where e.last_name like '%a';


/*5. Write a query to display the last name,job,department number and department name for 
all employees who work in ATLANTA.*/

select e.last_name,e.job_id,d.department_id,d.department_name,l.city from employees e join departments d
on d.department_id = e.department_id join locations l on d.location_id = l.location_id where l.city = 'Atlanta';

/*6. Display the employee last name and employee number along with their manager's last 
name and manager number.*/

select last_name,employee_id,manager_id,manager_lastName from employees;

/*7. Display the employee last name and employee number along with their manager's last 
name and manager number (including the employees who have no manager).*/

/*8. Create a query that displays employees last name,department number,and all the 
employees who work in the same department as a given employee.*/

select e.last_name,d.department_id,d.department_name from employees e join departments d
on d.department_id = e.department_id where e.employees_id =  [Givenemployees_id];

/*9. Create a query that displays the name,job,department name,salary,grade for all 
employees. Derive grade based on salary(>=50000=A, >=30000=B,<30000=C)*/

select e.first_name,e.job_id, d.department_name,e.salary,
case 
when e.salary>=50000 then 'A'
when e.salary>=30000 then 'B'
else 'C'
end as Grade
from employees e join departments d
 on d.department_id = e.department_id ;


/*10. Display the names and hire date for all employees who were hired before their 
managers along withe their manager names and hire date. Label the columns as Employee 
name, emp_hire_date,manager name,man_hire_date*/
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
-- ASSINGMENT 5
/* 
1. Write a query to display employee numbers and employee name (first name, last name) 
of all the sales employees who received an amount of 2000 in bonus.*/

select employee_id,first_name,last_name,commission_pct from employees where commission_pct ="2000"
and job_id = "SL_SALES";

/*2. Fetch address details of employees belonging to the state CA. If address is null, provide 
default value N/A.*/

SELECT E.COUNTRY_ID,location_ID,L.street_address,L.postal_code,L.city,L.state_province
FROM employees E JOIN LOCATIONS L ON E.COUNTRY_ID = L.COUNTRY_ID  group by 
E.country_id HAVING  E.COUNTRY_ID = "CA";

SELECT 
e.COUNTRY_id,L.street_address,e.first_name from 
 employees E JOIN LOCATIONS L ON E.COUNTRY_ID = L.COUNTRY_ID where e.country_id = 'CA'
 group by e.country_id,e.first_name,L.street_address;

SELECT @@GLOBAL.sql_mode = (select replace(@@sql_mode,'ONLY_FULL_GROUP_BY','ONLY_FULL_GROUP_BY'));
SET sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',""));


/*3. Write a query that displays all the products along with the Sales OrderID even if an order 
has never been placed for that product.*/
use hr;
create table products
(
product_id int ,
product_name varchar(200),
supplier_id int 
);
 create table sales_orders
 (
 order_id  int primary key,
 product_id int,
 OrderDate date,
 foreign key (product_id) references  products(product_id)
 );

 select p.product_id,p.product_name,coalesce(s.order_id,'No order')  from products p join sales_orders s on 
 p.product_id = s.product_id;
 
 /*4. Find the subcategories that have at least two different prices less than $15.*/
 use hr;
 create table categories 
 (
 categories_id int primary key,
 categories varchar(40),
 subcategories varchar(30),
 price int 
 );
 
 select subcategories ,price from categories where subcategories<15;
 
 
 SELECT
    c.subcategories,
    COUNT(DISTINCT c.Price) AS distinct_prices_count
FROM
    categories c
JOIN
    Products p ON p.categories_id = p.categories_Id
WHERE
    c.Price < 15
GROUP BY
   c.subcategories
HAVING
    COUNT(DISTINCT c.Price) >= 2;


/*5. A. Write a query to display employees and their manager details. Fetch employee id, 
employee first name, and manager id, manager name.
B. Display the employee id and employee name of employees who do not have manag*/ 
 -- A
 select e.employee_id,e.first_name,e.manager_id, m.manager_name from employees e left join newmanager m
 on e.manager_id = m.manager_id;
 
 -- b
 select employee_id, first_name from employees where manager_id is null;
 
/*6. A. Display the names of all products of a particular subcategory 15 and the names of their 
vendors.
B. Find the products that have more than one vendor.*/

-- A 
 SELECT P.PRODUCT_NAME,P.categories_id,C.SUBCATEGORIES,V.vendor_name FROM PRODUCTS P JOIN categories c
 ON P.categories_id = C.categories_id JOIN  VENDOR V ON  C.VENDOR_ID = V.VENDOR_ID WHERE c.categories_id ='12';
 -- B

 select p.product_name,pv.vendor_id,count(pv.product_id) from products p join 
 Product_Vendors pv on p.product_id = pv.product_id group by pv.product_id having product_id>1;
 
 
CREATE TABLE Product_Vendors (
    product_id INT,
    vendor_id INT,
    PRIMARY KEY (product_id, vendor_id),
    FOREIGN KEY (product_id) REFERENCES Products(product_id),
    FOREIGN KEY (vendor_id) REFERENCES Vendor(vendor_id)
);
 
/*7. Find all the customers who do not belong to any store.*/ 

create database Commerse;
use Commerse;

use commerse;
create table customers
(
	customer_id int primary key,
    c_name varchar(20),
    phone_no int
);

use commerse;
create table store
(
store_id int primary key,
store_name varchar(50),
location varchar(50)
);
SELECT C.customer_id,C.C_NAME,S.STORE_NAME FROM CUSTOMERS C LEFT JOIN STORE S ON C.STORE_ID = S.STORE_ID
WHERE STORE_NAME IS NULL ; 


/*8. Find sales prices of product 718 that are less than the list price recommended for that 
product


-- list price > store
-- sales price > customer
*/
select product_id ,sales_price from products where sales_price <list_price and  product_id = '718';

/*9. Display product number, description and sales of each product in the year 2001.*/

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
-- Assingment 6

/*1. Write a query to display the last name and hire date of any employee in the same 
department as SALES.*/
select last_name,hire_date,job_id from employees where job_id = "sl_sales";

/*2. Create a query to display the employee numbers and last names of all employees who 
earn more than the average salary. Sort the results in ascending order of salary.*/

select round (avg(salary)) from employees;
-- avg salary is 9429
select employee_id,last_name from employees 
where salary >(SELECT round (avg(salary))FROM EMPLOYEES) order by salary
asc ;

/*3. Write a query that displays the employee numbers and last names of all employees who 
work in a department with any employee whose last name contains a' u*/

select employee_id,last_name,salary from employees where 
last_name like "%a' u%";
-- wildcard funtion %%


/*4. Display the last name, department number, and job ID of all employees whose 
department location is ATLANTA.*/

select e.last_name,e.department_id,e.job_id from employees e join locations l on
l.country_id = e.country_id where l.city = "ATLANTA";

/*5. Display the last name and salary of every employee who reports to FILLMORE.*/

SELECT last_name, salary
FROM employees
WHERE manager_name = (SELECT employee_id FROM employees WHERE last_name = 'Christian');

/*6. Display the department number, last name, and job ID for every employee in the 
OPERATIONS department.*/

select d.department_id,e.last_name,e.job_id  from employees e join departments d
on d.department_id = e.department_id where d.department_name = 'OPERATIONS';

/*7. Modify the above query to display the employee numbers, last names, and salaries of all 
employees who earn more than the average salary and who work in a department with any 
employee with a 'u'in their name*/

select employee_id,last_name ,salary from 
employees where salary>(select avg(salary)) and last_name like "%a' u%";


SELECT @@GLOBAL.sql_mode = (select replace(@@sql_mode,'ONLY_FULL_GROUP_BY','ONLY_FULL_GROUP_BY'));
SET sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',""));


/*8. Display the names of all employees whose job title is the same as anyone in the sales 
dept.*/

select first_name,job_id from employees where job_id ='SL_SALES';

/*9. Write a compound query to produce a list of employees showing raise percentages, 
employee IDs, and salaries. Employees in department 1 and 3 are given a 5% raise, 
employees in department 2 are given a 10% raise, employees in departments 4 and 5 are 
given a 15% raise, and employees in department 6 are not given a raise.*/

/*9. Write a compound query to produce a list of employees showing raise percentages, 
employee IDs, and salaries. Employees in department 10 and 30 are given a 5% raise, 
employees in department 20 are given a 10% raise, employees in departments 40 and 50 are 
given a 15% raise, and employees in department 60 are not given a raise.*/
-- here in this query first i have used = in when clause to specifie the department, but it was a wrong synetex
-- i must use 'in' in place of '='.
-- incorrect 
select e.salary,e.employee_id ,d.department_id ,
case 
when d.department_id = '10' and '30' then salary + salary * 5%
when d.department_id ='40' and '50' then salary + salary * 15%
when d.department_id ='20' then salary + salary * 10%
else 'no raise'
end as raise percentages
from employees e join departments d
on e.department_id = d.department_id ;

-- correct one
select e.salary,e.employee_id ,d.department_id ,
case 
when d.department_id in ('10','30') then salary + salary * 0.05
when d.department_id in ('40','50') then salary + salary * 0.15
when d.department_id = ('20') then salary + salary * 0.10
else 'no raise'
end as raise_percentages
from employees e join departments d
on e.department_id = d.department_id ;

/*10. Write a query to display the top three earners in the EMPLOYEES table. Display their last 
names and salaries.*/

select last_name,salary from employees order by salary desc 
limit 3;

/*11. Display the names of all employees with their salary and commission earned. Employees 
with a null commission should have O in the commission column*/

		select first_name,salary,
        case 
        when commission_pct is not null then commission_pct
        else commission_pct is null = '0'
        end as commission_earned
        from employees;													
	
/*12. Display the Managers (name) with top three salaries along with their salaries and 
department information.*/
    
    select manager_name,salary ,department_id from employees order by salary desc limit 3 ;
    
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Assingment 7
/*
1) Find the date difference between the hire date and resignation_date for all the 
employees. Display in no. of days, months and year(1 year 3 months 5 days).
Emp_ID Hire Date Resignation_Date
1 1/1/2000   7/10/2013
2 4/12/2003  3/8/2017
3 22/9/2012  21/6/2015
4 13/4/2015  NULL
5 03/06/2016 NULL
6 08/08/2017 NULL
7 13/11/2016 NULL
*/
use hr;
create table EMP_Date
(
employee_id int primary key,
hire_date date,
resignation_date date);


select datediff("2000-01-01" , "2023-10-07") FROM emp_date;



SELECT
    employee_id,
    Hire_Date,
    Resignation_Date,
    IFNULL(DATEDIFF(Resignation_Date, Hire_Date), DATEDIFF(CURRENT_DATE(), Hire_Date)) AS DaysDifference,
    CONCAT(
        FLOOR(IFNULL(DATEDIFF(Resignation_Date, Hire_Date), DATEDIFF(CURRENT_DATE(), Hire_Date)) / 365), ' years ',
        FLOOR((IFNULL(DATEDIFF(Resignation_Date, Hire_Date), DATEDIFF(CURRENT_DATE(), Hire_Date)) % 365) / 30.44), ' months ',
        (IFNULL(DATEDIFF(Resignation_Date, Hire_Date), DATEDIFF(CURRENT_DATE(), Hire_Date)) % 365) % 30, ' days'
    ) AS FormattedDifference
FROM
    emp_date;

/*2) Format the hire date as mm/dd/yyyy(09/22/2003) and resignation_date as mon dd, 
yyyy(Aug 12th, 2004). Display the null as (DEC, 01th 1900)*/

select
	   ifnull(date_format(hire_date, ' %c/%d/%Y'),'%b D %Y') as Hire_dateFOrmated,
       ifnull( date_format(resignation_date,' %b/%D/%Y'),'Dec, 010th 1900') as resignation_date
from emp_date;

/*3) Calcuate experience of the employee till date in Years and months(example 1 year and 3 
months)*/

select employee_id ,current_date() as  currentDate,
concat(
 floor( timestampdiff(year,hire_date,current_date())),' ','years',' ','and',' ', 
timestampdiff(month,hire_date,current_date())%12, 'months')as experiance 
from emp_date;

/*4) Display the count of days in the previous quarter */

SELECT
    COUNT(*) AS days_in_previous_quarter

WHERE
	 DATE_SUB(DATE_FORMAT(CURRENT_DATE, '%Y-%m-01'), INTERVAL 3 MONTH)
    AND current_date() < DATE_FORMAT(CURRENT_DATE, '%Y-%m-01');


/*5) Fetch the previous Quarter's second week's first day's date*/
select 
concat(
extract(day from dayofweek(date_sub(current_date(),interval 3 month)))
 );

select date_sub(current_date(), interval 3 QUARTER); -- 2023-09-18

select extractvalue( '2023-09-18');


SELECT 
    DATE_ADD(
        LAST_DAY(DATE_SUB(CURDATE(), INTERVAL 1 QUARTER)),
        INTERVAL 1 DAY
    ) AS FirstDayOfSecondWeekOfPreviousQuarter;


SELECT 
    DATE_ADD(
        DATE_SUB(LAST_DAY(DATE_SUB(CURDATE(), INTERVAL 1 QUARTER)), INTERVAL 1 MONTH),
        INTERVAL 1 DAY
    ) AS FirstDayOfSecondMonthOfPreviousQuarter;


/*6) Fetch the financial year's 15th week's dates (Format: Mon DD YYYY)*/

/*7) Find out the date that corresponds to the last Saturday of January, 2015 using with 
clause.*/
 
/*Use Airport database for the below two question:

8) Find the number of days elapsed between first and last flights of a passenger.

9) Find the total duration in minutes and in seconds of the flight from Rostov to Moscow.
*/
    --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    

    
