# SQL-Practise

create database ORGN;
SHOW databases;
use ORGN;

CREATE TABLE Worker(
Worker_ID INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
FIRST_NAME CHAR(25),
LAST_NAME CHAR(25),
SALARY INT(15),
JOINING_DATE DATETIME,
DEPARTMENT CHAR(25)
);

INSERT INTO Worker
(WORKER_ID, FIRST_NAME, LAST_NAME, SALARY, JOINING_DATE, DEPARTMENT) values
(001,'Monika','Arora','100000','14-02-20 09:00:00','HR'),
(002,'umsh','Arora','10000','14-02-20 09:00:00','Admin'),
(003,'Moka','Arora','1000000','14-02-20 09:00:00','HR'),
(004,'roy','Arora','10000000','14-02-20 09:00:00','Admin'),
(005,'umesh','Arora','100','14-02-20 09:00:00','Admin'),
(006,'vinay','Arora','10000','14-02-20 09:00:00','Account'),
(007,'dilip','Arora','100000','14-02-20 09:00:00','Account'),
(008,'deepak','Arora','500000','14-02-20 09:00:00','Admin');
UPDATE Worker
SET SALARY = '2000000'
WHERE WORKER_ID = 001;
 
create table Bonus (
WORKER_REF_ID INT,
BONUS_AMOUNT INT(10),
BONUS_DATE DATETIME,
foreign key (WORKER_REF_ID)
		REFERENCES Worker(WORKER_ID)
        ON DELETE CASCADE
);

insert into Bonus
  (WORKER_REF_ID, BONUS_AMOUNT, BONUS_DATE) VALUES
	(001,5000,'16-02-20'),
    (002,3000,'16-02-11'),
    (003,4000,'16-02-20'),
    (001,4500,'16-02-20'),
    (002,3500,'16-02-11');
    

	-- Step 1: Create the table
CREATE TABLE Title (
    WORKER_REF_ID INT,
    WORKER_TITLE CHAR(25),
    AFFECTED_FORM DATETIME
);

-- Step 2: Add the foreign key constraint
ALTER TABLE Title
ADD CONSTRAINT fk_worker_ref_id
FOREIGN KEY (WORKER_REF_ID)
REFERENCES Worker(WORKER_ID)
ON DELETE CASCADE;

insert into Title
    (WORKER_REF_ID, WORKER_TITLE, AFFECTED_FORM) VALUES
		(001,'Manager','2015-02-20 00:00:00'),
		(002,'Executive','2015-02-20 00:00:00'),
        (008,'Executive','2015-02-20 00:00:00'),
        (005,'Manager','2015-02-20 00:00:00'),
        (004,'Asst. Manager','2015-02-20 00:00:00'),
		(007,'Executive','2015-02-20 00:00:00'),
        (006,'Lead','2015-02-20 00:00:00'),
        (003,'Lead','2015-02-20 00:00:00');
        
        -- 1 for first name as worker_name 
        select first_name as Worker_name from worker;
        
        -- 2 for UPPER Case
        select upper(first_name) from worker;
        
        -- 3 for unique values 
        select distinct department from worker;
        
        -- first 3 word
        
        select substring(first_name,1,3) from worker;
        
        
        -- 4 occurance of b from amitabh
        
        select instr(first_name,'a') from worker where first_name='monika';
        
        --  5right spaces remove and print first name
        
        select RTRIM(first_name) from worker;
        
        -- 7 remove space from left and print department 
        
        -- 8 unique value + length
        select distinct department, LENGTH(department) from worker;
         
        -- 9 replace a with A then print
        select replace(first_name,'a','A') from worker;
        
        -- 10 print first and last name with space
        select concat(first_name,' ',last_name) from worker;
        
        -- 11 print first name in assending order
        
        select * from worker order by first_name; 
        
        -- 12 first name assending department descending
        
        select * from worker order by first_name, department desc;
        
        -- 13 print particular name value data
        
        select * from worker where first_name in ('moka', 'monika');

		 -- 14 print particular name ko chodkar data 
         
         select * from worker where first_name not in ('moka', 'monika');
        
        	 -- 15 admin vale department name ke worker
		select * from worker where department in ('Admin');
        
         -- 15 admin vale department name ke worker admin ke aage kuchh or bhi ho sakta hai
        select * from worker where department like 'Admin%';
        
        -- 16 a contain krne vale worker ki details
        select * from worker where first_name like '%a%';
        
        -- 17 sql query first name end with a 
          select * from worker where first_name like '%a';
          
           -- 18 sql query first name end with a and contain 6 alphabets
           
           select * from worker where first_name like '_____a';
           
           -- 19 write an query whose salary between 100000 and 500000
           
           select * from worker where salary between 100000 and 500000;
        
        -- 20 print details of worker who join in feb 2014
        
        select * from worker where year(joining_date) = 2014 and month(joining_date) = 02;
        
        -- 21 print the count of particular department
        
        select department , count(*) from worker where department='Admin';
        
        -- full name with salray range 50000 to 100000
        
        select concat(first_name,' ',last_name) as full_name from worker where salary between 50000 and 100000;
	-- 23 no. of worker from each department in descending order
    
    select department, count(worker_id) from worker group by department order by count(worker_id) desc;
    
    -- 24 worker who are manager inner join
    select w.* from worker as w inner join title as t on w.worker_id=t.worker_ref_id where t.worker_title='Manager'; 

-- 25 fetch number of same title in the org of different types 1 se jyada ko print krna hai

select worker_title, count(*) from title group by worker_title having count(*) >1;

 -- 26  write an query to show only odd rows from table
 
select * from worker where mod (worker_id,2) !=0;

-- <> this behave like not equal to 
select * from worker where mod (worker_id,2) <>0;

 -- 27  write an query to show only even rows from table
 
select * from worker where mod (worker_id,2) =0;

-- 28 write an sql query to clone a new table from another table
create table worker_clone like worker;
insert into worker_clone select * from worker;
select * from worker_clone;

-- 29 intersection in two table commun values
select worker.* from worker inner join title using(worker_id);
-- 30 show recordfrom one table that another table does not have

select * from worker left join worker_clone using(worker_id) where worker_clone.worker_id is null;

-- 31 write an sql query to show the current date and time

select curdate();
select now();

-- 32 top n salary n=5 sbse jyada ke liye 1 kr denge

select * from worker order by salary desc limit 5;

-- 33 determine the nth highest salary n=5 so  5 vi highest salary
select * from worker order by salary desc limit 4,1;

-- 34 determine the nth highest salary n=5 so  5 vi highest salary without using LIMIT keyword
SELECT * FROM worker w1
 WHERE 4 = (SELECT COUNT(DISTINCT (w2.salary)) 
 FROM worker w2 
 WHERE w2.salary >= w1.salary);

-- 35 fetch the list of employee with the same salry
select w1.* from worker w1 , worker w2 where w1.salary = w2.salary and w1.worker_id != w2.worker_id;

-- 36 write a query to show the second hisghest salary from a table using sub-query

select max(salary) from worker ;
 -- for maximum salary

select max(salary) from worker
where  salary not in (select max(salary) from worker);

-- 37 write an sql query to show one row twice in result from a table one row two time


select * from worker 
union all
select * from worker order by worker_id;

-- 38 worker id who does not get bonus

select worker_id from worker
 where worker_id not in (select worker_ref_id from bonus);
 
 -- 39 write an sql query to fetch 50% records from a table
 select * from worker
 where worker_id <= (select count(worker_id)/2 from worker);
 
 -- 40 write an sql query to fetch department that have less than 4 people in it.

	select department, count(department) as depCount from worker group by department having depCount<4;

  -- 41 show all department along  with number
  select department, count(department)  as depCount from worker group by department;

-- 42 show the last record from a table 
select * from worker where worker_id = (select max(worker_id) from worker);

-- 43 write an sql query to fetch the first row of table
select * from worker where worker_id = (select min(worker_id) from worker);

-- for aximu salary with all details
select * from worker where SALARY = (select max(salary) from worker);
  
  -- 44 write an sql query to fetch the last five records from a table
  
  select * from worker order by worker_id desc limit 5; 
  
  -- 45 write an sql query to print the name of employee having the highest salry in each department
select max(salary) as maxSal, department from worker group by department;   
  
  select distinct w.department, w.first_name, w.salary from
  (select distinct max(salary) as maxSal, department from worker group by department) temp
inner join worker w on temp.department = w.department and temp.maxSal = w.salary;  

-- 46 write an sql query to fetch three max salaries from a table using co-related sub-queries
select distinct salary from worker w1 
where 3>= (select count(distinct salary) from worker w2 where w1.salary <= w2.salary) order by w1.salary desc;
  
  select distinct salary from worker order by salary desc limit 3;
  
  -- 47 write an sql query to fetch 3 minimum salary 
  select distinct salary from worker w1 
where 3>= (select count(distinct salary) from worker w2 where w1.salary >= w2.salary) order by w1.salary desc;
  
  -- 48 write an sql query to fetch departments along with the total salaries paid for each of them.
  
  select distinct salary from worker w1 
where 3>= (select count(distinct salary) from worker w2 where w1.salary <= w2.salary) order by w1.salary desc;
  
  -- 49 write an sql query to fetch department along with the total salaries paid for each of them
  
  select department, sum(salary) as depSal from worker group by department;
  
  -- 50 write an sql query to fetch the names of worker who earn the highest salary
  select first_name, salary from worker where salary = (select max(Salary) from worker); 
  
select * from worker;

