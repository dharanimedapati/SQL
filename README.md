# SQL
POSTGRESQL

// select

Select * from actor;
Select first_name, last_name from actor;
Select last_name,  first_name from actor;

Select * from city;


#challenge

Select first_name, last_name , email from customer;


//distinct

select distinct name from color_table;

select * from film;

select distinct release_year from film;

select distinct (release_year) from film;

select distinct (rental_rate) from film;

//challenge
select distinct (rating) from film;


//count

select count(name) from table;
select count(choise) from table;
select count(*) from table;


select * from payment;

select count(*) from payment;
select count(amount) from payment;


select count(distinct(amount)) from payment;


//Where

select * from customer 
where first_name = 'Jared';

Select * from film 
Where rental_rate > 4 AND replacement_cost >= 19.99
AND rating = 'R';

Select title from film 
Where rental_rate > 4 AND replacement_cost >= 19.99
AND rating = 'R';

--count
Select count(title) from film 
Where rental_rate > 4 AND replacement_cost >= 19.99
AND rating = 'R';

select count (*) from film
Where rating = 'R' OR rating = 'PG-13';


Select * from film
Where rating != 'R';

--where challenge , what is the email for the customer named nancy thomas

Select * from customer;

select email from customer
Where First_name = 'Nancy' AND last_name = 'Thomas';


--challenge 2 , Acustomer wants to what the movie'outlaw hanky' is about , could you give them the description of the movie 


select * from film;

select description from film
Where title = 'Outlaw Hanky';


--order by 

select coulmn_1 , column_2
from table
order by column_1 ASC/DESC;

select * from customer
order by first_name;

select store_id, first_name, last_name from customer
order by store_id DESC , first_name ASC;

--limit

Select * from payment 
where amount != 0.00
order by payment_date  DESC
limit 5;

-- challenge, what are the customer IDs of the first 10 customers who created a payment 
Select customer_id from payment 
where amount != 0.00
order by payment_date  ASC
limit 10;

--what are the titles sof the 5 shortest movies(in length of runtime)?

select title,length from film
order by length ASC
limit 5;

select count(title) from film
Where length <= '50';


--Between

select count(*) from payment
where amount not between 8 and 9 ;

select count(*) from payment
where payment_date between '2007-02-01' and '2007-02-15' ;

--IN

select color from table
where color IN('Red''Blue');


select count(*) from payment 
where amount IN('0.99','1.98','1.99');


select count(*) from payment 
where amount NOT IN('0.99','1.98','1.99');

select * from customer
where first_name IN ('John','Jake','Julie');

--like and Ilike 

select count(*) from customer
where first_name LIKE 'J%' AND last_name LIKE 'S%'

select count(*) from customer
where first_name LIKE '%er%'

select * from customer
where first_name LIKE '_her%'


-- challenges 1) how many payment transactions are greater than 5.00?

select count(amount) from payment 
where amount > 5.00;

--2)How many actors have their first name starts with P ?

Select count(*) from actor
where first_name Like 'P%';

--3) how many unique districts are our customers from?

select count(distinct (district)) from address;


-- retrieve the list of names for those distinct districts 

select distinct(district) from address
order by district;

--how many films have a rating of R and a replacement cost  between 5 and 15
select count(*) from film
where rating = 'R' AND replacement_cost between 5 AND 15;

--how many films have the word TRUMAN somewhere in the title

select count(*) from film
where title like '%Truman%'


--Aggregate functions

select min(replacement_cost) from film;

select max(replacement_cost) from film;

select min(replacement_cost),max(replacement_cost) from film;

select count(*) from film;

select ROUND(AVG(replacement_cost),4) from film;

select SUM(replacement_cost) from film;


--
Select catogery_column,agg(data_column) 
from table 
where catogery_column != A 
group by catogery_column;

select company,division,sum(sales)
from finance_table
where division in ('marketing','transport')
group by company,division

--challenge
select * from payment ;

select staff_id,count(*) from payment
group by staff_id

--challenge

select rating,ROUND(AVG(replacement_cost),2)from film
group by rating;

--challenge 

select * from payment;
select customer_id,sum(amount) from payment
group by customer_id 
order by sum(amount) DESC
limit 5


select customer_id,count(amount) from payment
group by customer_id
having count(amount) >= 40 

select customer_id , sum(amount) from payment 
where staff_id ='2'
group by customer_id 
having sum(amount) > 100

-- 1. Return the customer IDs of customers who have spent at least $110 with the staff member who has an ID of 2.

--The answer should be customers 187 and 148.

select customer_id from payment
where staff_id = '2' 
Group by customer_id
Having sum(amount) >= 110 



--2. How many films begin with the letter J?

--The answer should be 20.

select count(title) from film 
where title like 'J%'


--3. What customer has the highest customer ID number whose name starts with an 'E' and has an address ID lower than 500?

--The answer is Eddie Tomlin

select first_name, last_name  from customer 
where first_name Like 'E%' And address_id < 500
order by customer_id DESC
LIMIT 1;

--challenge

select district,email from address
INNER JOIN customer
ON address.address_id = customer.address_id
where district = 'California';

SELECT title, first_name, last_name
FROM film_actor
INNER JOIN film ON film_actor.film_id = film.film_id
INNER JOIN actor ON film_actor.actor_id = actor.actor_id
WHERE first_name = 'Nick' AND last_name = 'Wahlberg';

--
CREATE TABLE account (
    user_id     SERIAL PRIMARY KEY,
    username    VARCHAR(50) UNIQUE NOT NULL,
    password    VARCHAR(50) NOT NULL,
    email       VARCHAR(250) UNIQUE NOT NULL,
    created_on  TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    last_login  TIMESTAMP
);

CREATE TABLE job (
    job_id   SERIAL PRIMARY KEY,
    jobname  VARCHAR(2000) UNIQUE NOT NULL
);

CREATE TABLE account_job (
    user_id   INTEGER NOT NULL REFERENCES account(user_id) ON DELETE CASCADE,
    job_id    INTEGER NOT NULL REFERENCES job(job_id) ON DELETE CASCADE,
    hiredate  TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (user_id, job_id)  -- Composite primary key
);


--insert

INSERT INTO account (username, password, email, created_on, last_login) VALUES
('jdoe', 'password123', 'jdoe@example.com', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP),
('asmith', 'securepass', 'asmith@example.com', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP),
('bjohnson', 'mypassword', 'bjohnson@example.com', CURRENT_TIMESTAMP, NULL);

INSERT INTO job (jobname) VALUES
('Software Engineer'),
('Data Analyst'),
('Project Manager');

INSERT INTO account_job (user_id, job_id, hiredate) VALUES
(1, 1, CURRENT_TIMESTAMP),  -- jdoe hired as Software Engineer
(2, 2, CURRENT_TIMESTAMP),  -- asmith hired as Data Analyst
(3, 3, CURRENT_TIMESTAMP),  -- bjohnson hired as Project Manager
(1, 2, CURRENT_TIMESTAMP);  -- jdoe also works as Data Analyst


--update 
UPDATE account
SET email = 'john.doe@newmail.com',
    last_login = CURRENT_TIMESTAMP
WHERE username = 'jdoe';

UPDATE job
SET jobname = 'Senior Software Engineer'
WHERE jobname = 'Software Engineer';

UPDATE account_job
SET hiredate = '2023-01-15 09:00:00'
WHERE user_id = 1 AND job_id = 1;


--assessment



-- Step 3: Create the students table
CREATE TABLE students (
    student_id       SERIAL PRIMARY KEY,
    first_name       VARCHAR(50) NOT NULL,
    last_name        VARCHAR(50) NOT NULL,
    homeroom_number  INTEGER NOT NULL,
    phone            VARCHAR(15) UNIQUE NOT NULL,
    email            VARCHAR(100) UNIQUE,
    graduation_year  INTEGER NOT NULL
);

-- Step 4: Create the teachers table
CREATE TABLE teachers (
    teacher_id       SERIAL PRIMARY KEY,
    first_name       VARCHAR(50) NOT NULL,
    last_name        VARCHAR(50) NOT NULL,
    homeroom_number  INTEGER NOT NULL,
    department       VARCHAR(100) NOT NULL,
    email            VARCHAR(100) UNIQUE NOT NULL,
    phone            VARCHAR(15) UNIQUE NOT NULL
);

-- Step 5: Insert student Mark Watney
INSERT INTO students (student_id, first_name, last_name, homeroom_number, phone, graduation_year)
VALUES (1, 'Mark', 'Watney', 5, '777-555-1234', 2035);

-- Step 6: Insert teacher Jonas Salk
INSERT INTO teachers (teacher_id, first_name, last_name, homeroom_number, department, email, phone)
VALUES (1, 'Jonas', 'Salk', 5, 'Biology', 'jsalk@school.org', '777-555-4321');
