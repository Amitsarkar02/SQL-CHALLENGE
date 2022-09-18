# SQL-CHALLENGE
SQL 10 DAY CHALLENGE

DAY 1
Question: Find the top countries with the 4.9 rating businesses. Output the
country name along with the number of '4.9' rated businesses and order records
by the City names in descending order.
In case there are two countries with the same result, sort them in alphabetical

QUERY-
SELECT Country, City, Count(Rating) as NoOfRating
FROM Zomato z
INNER/LEFT/RIGHT JOIN Country_details c on ‘c.Country Code’=z.CountryCode
WHERE Rating=4.9
GROUP BY Country
ORDER BY Country, City Desc;

  
  
  
DAY 2
QUESTION - Ivy Pro School wants you to work on a Project data that has a table. The table has
three columns-Task_ID,Start_Date and End_Date.
It is guaranteed that the difference between the End_Date and the Start_Date is equal
to 1 day for each row in the table.
If the End_Date of the tasks are consecutive, then they are part of the same project.
Ivy is interested in finding the total number of different projects completed.  Write a query to output the start and end dates of projects listed by the number of
days it took to complete the project in ascending order.  If there is more than one project that have the same number of completion days,
then order by the start date of the project.

QUERY-CREATE table Project (
Task_Id int,
Start_Date date,
End_Date date);

INSERT INTO Project (Task_Id, Start_Date, End_Date)
Values
("1", "2015-10-01", "2015-10-02"),
("2", "2015-10-02", "2015-10-03"),
("3", "2015-10-03", "2015-10-04"),
("4", "2015-10-13", "2015-10-14"),
("5", "2015-10-14", "2015-10-15"),
("6", "2015-10-28", "2015-10-29"),
("7", "2015-10-30", "2015-10-31");
SELECT * FROM Project;
SELECT Start_Date, MIN(End_Date) as End_Date
FROM(
SELECT Start_Date
FROM Project
WHERE Start_Date NOT IN(
SELECT End_Date FROM Project)) p1,
(SELECT End_Date
FROM Project
WHERE End_Date NOT IN(
SELECT Start_Date FROM Project))p2
WHERE Start_Date<End_Date
GROUP BY Start_Date
ORDER BY datediff(Min(End_Date),Start_Date),Start_Date ASC\



DAY 3
Write an SQL query to report the Id and Names of customers who bought products

create table customers(
customer_id int,
customer_name varchar(100),
constraint c_pk primary key(customer_id));

create table orders(
order_id int,
customer_id int,
product_name varchar(100),
constraint o_pk primary key(order_id));

select * from customers;
select * from orders;

insert into customers values(1,"steve rogers");
insert into customers values(2,"Bruce Banner");
insert into customers values(3,"Tony Stark");
insert into customers values(4,"tom holland");

insert into orders values(10,1,'A');
insert into orders values(20,1,'B');
insert into orders values(30,1,'D');
insert into orders values(40,2,'C');
insert into orders values(50,2,'B');
insert into orders values(60,2,'B');

insert into orders values(70,3,'A');
insert into orders values(80,3,'C');
insert into orders values(90,4,'C');
insert into orders values(100,4,'D');

select *
from customers
where customer_id in
( select distinct customer_id
from orders
where product_name = 'C'
) and
customer_id in
(select distinct customer_id
from orders
where product_name = 'B'
) and
customer_id not in
(select distinct customer_id
from orders
where product_name = 'A')

order by customer_id;



DAY 4
1 A 1
2 A 2
3 A 3
4 A 5
5 A 6
6 A 8
7 A 9
8 B 11
9 C 1
10 C 2
11 C 3

QUESTION
Write a SQL query to find the maximum and minimum values of continuous
‘Sequence’ in each ‘Group’.

QUERY
create table Meta(
sl_no int,
Group_name varchar(100),
sequence int);

insert into meta values(1,'A',1);
insert into meta values(2,'A',2);
insert into meta values(3,'A',3);
insert into meta values(4,'A',5);
insert into meta values(5,'A',6);
insert into meta values(6,'A',8);
insert into meta values(7,'A',9);
insert into meta values(8,'B',11);
insert into meta values(9,'C',1);
insert into meta values(10,'C',2);
insert into meta values(11,'C',3);

select * from meta;

select sl_no,group_name,
min(sequence) as min_sequence,
max(sequence) as max_sequence
from
(
select sl_no,group_name,sequence,
sequence-ROW_NUMBER() OVER (PARTITION BY group_name ORDER BY Sequence) rn
FROM meta

)A
group by group_name
order BY group_name;



DAY 5
QUESTION
You are given three tables: Students, Friends and Packages.
Students contains two columns: ID and Name.
Friends contains two columns: ID and Friend_ID (ID of the ONLY friend).
Packages contains two columns: ID and Salary (offered salary in $ thousands per month).

Write a query to output the names of those students whose friends got offered a higher
salary than them. Names must be ordered by the salary amount offered to the friends. It is
guaranteed that no two students got same salary offer.

QUERY
create table students(
ID int,
Name varchar(100));

create table friends(
ID int,
friend_id int);

create table packages(
ID int,
packages float);

insert into students values(1,'Ashley');
insert into students values(2,'samantha');
insert into students values(3,'julia');
insert into students values(4,'scarlet');

insert into friends values(1,2);
insert into friends values(2,3);
insert into friends values(3,4);
insert into friends values(4,1);

insert into packages values(1,15.20);
insert into packages values(2,10.06);
insert into packages values(3,11.55);
insert into packages values(4,12.12);

select * from students;
select * from friends;
select * from packages;

select s.name
from students s inner join packages as p1 on s.ID=p1.ID
join friends as f on s.id=f.id
join students as s2 on f.friend_id=s2.ID
join packages as p2 on s2.ID=p2.ID
WHERE p1.packages < p2.packages
ORDER BY p2.packages;



DAY 6 
QUESTION
Write a query to print the sum of total investment values in 2016
(TIV_2016), to a scale of 2 decimal places, for the policy holders who meet
the following criteria:
1.Have the same TIV_2015 value as one or more other policyholders.
2. Are not located in the same city as any other policyholder (i.e.: the
(latitude, longitude) attribute pairs must be unique). Input Format: The
insurance table is described as follows:
PID is the policyholder’s policy ID,
TIV_2015 is the total investment value in 2015,
TIV_2016 is the total investment value in 2016,
LAT is the latitude of the policy holder’s city, and
LON is the longitude of the policy holder’s city.

QUERY
CREATE DATABASE Investment;
USE Investment;
CREATE table Insurance (
PID int,
TIV_2015 int,
TIV_2016 int,
LAT int,
LON int);

Insert into Insurance
Values (1, 10, 5, 10, 10),
(2, 20, 20, 20, 20),
(3, 10, 30, 20, 20),
(4, 10, 40, 40, 40);

SELECT * FROM Invest;

SELECT SUM (TIV_2016) AS TIV_2016
FROM Insurance
WHERE CONCAT (LAT, ',', LON)
IN (SELECT CONCAT (LAT, ',', LON)
FROM Insurance
GROUP BY LAT, LON
HAVING COUNT (1) = 1)
AND TIV_2015 IN
(SELECT TIV_2015
FROM Insurance
GROUP BY TIV_2015
HAVING COUNT (1)>1);



DAY 7
student_id int
subject string
marks int
Input Table:
student_id subject marks
1001 English 88
1001 Science 90
1001 Maths 85
1002 English 70
1002 Science 80
1002 Maths 83
Problem Statement:
Reproduce the Table into given format below:
Output:
student_id English Science Maths
1001 88 90 85
1002 70 80 83

QUERY
create table marks_data(
Student_id int,
subject varchar(100),
marks int);

insert into marks_data values(1001,'English',88);
insert into marks_data values(1001,'Science',90);
insert into marks_data values(1001,'Maths',85);
insert into marks_data values(1002,'English',70);
insert into marks_data values(1002,'Science',80);
insert into marks_data values(1002,'Maths',83);

select Student_id,
sum(case when Subject= 'English' then Marks else 0 end) as English,
sum(case when Subject= 'Science' then Marks else 0 end) as Science,
sum(case when Subject= 'Maths' then Marks else 0 end) as Maths
from marks_data group by Student_id order by student_id;

select * from marks_data;



DAY 8
QUESTION
person_id is the primary key column for this table.
This Table has the information about all people waiting for an elevator.
The person_id and turn columns will contain all numbers from 1 to n,
where n is the number of rows in the table.
The maximum weight the elevator can hold is 1000.
Write an SQL query to find the person_name of the last person who will fit
in the elevator without exceeding the weight limit.
It is guaranteed that the person who is first in the Queue can fit in the elevator.

QUERY
create table elevator(
person_id int primary key,
person_name varchar(100),
weight int,
turn int);

insert into elevator values(5,'George Washington',250,1);
insert into elevator values(3,'john adams',350,2);
insert into elevator values(6,'Thomas jefferson',400,3);
insert into elevator values(2,'Will johnliams',200,4);
insert into elevator values(4,'Thomas jefferson',175,5);
insert into elevator values(1,'James elephant',500,6);

select * from elevator;

select a.person_name
from elevator a, elevator b
where a.turn >= b.turn
group by a.person_id
having sum(b.weight) <= 1000
order by a.turn desc
limit 1;



DAY 9 
QUESTION
(player_id, event_date) is the primary key of this table.
This Table shows the Activity of players of some game.
Each row is a record of a player who logged in and played a number of games
(possibly 0) before logging out on some day using some device.
We define the install date of a player to be the first login day of that player.
We also define day 1 retention of some date X to be the number of players
whose install date is X and they logged back in on the day right after X,
divided by the number of players whose install date is X, rounded to 2 decimal places.
Write an SQL query that reports for each install date, the number of players that installed
the game on that day and the day 1 retention.

QUERY
with t1 as(
select *,
row_number() over(partition by player_id order by event_date) as rnk,
min(event_date) over(partition by player_id) as install_dt,
lead(event_date,1) over(partition by player_id order by event_date) as nxt
from Activity)
select distinct install_dt,
count(distinct player_id) as installs,
round(sum(case when nxt=event_date+1 then 1 else 0 end)/count(distinct player_id),2)
as Day1_retention
from t1
where rnk = 1
group by 1
order by 1



DAY10
QUESTION
An IT Company interviews candidates using coding challenges and contests. Write a query to print the contest_id , hacker_id , name , and the sums of total_submissions,
total_accepted_submissions, total_views, and total_unique_views for each contest sorted by contest_id. Exclude the contest from the result if all four sums are 0 .
Note: A specific contest can be used to screen candidates at more than one college, but each college only holds screening contest.

EXPLANATION
The contest 66406 is used in the college 11219 . In this college 11219 , challenges 18765 and 47127
are asked, so from the view and submission stats:
1.Sum of total submissions =27+56+28=111
2.Sum of total accepted submissions =10+18+11=39
3.Sum of total views =43+72+26+15=156
4.Sum of total unique views =10+13+19+14=56
Similarly, we can find the sums for contests 66556 and 94828.

QUERY

create table contest(
contest_id int,
hacker_id int,
Name varchar(100));

create table colleges(
college_id int,
contest_id int);

create table challenges(
challenge_id int,
college_id int);

create table stats(
challenge_id int,
total_views int,
total_unique_views int);

create table submission_stats(
challenge_id int,
total_submissions int,
total_accepted_submissions int);

insert into contest values(66406,17973,'Rose');

insert into contest values(66556,79173,'Angela');
insert into contest values(94828,80275,'Frank');

select * from contest;

insert into colleges values(11219,66406);
insert into colleges values(32473,66556);
insert into colleges values(56685,94828);

select * from colleges;

insert into challenges values(18765,11219);
insert into challenges values(47127,11219);
insert into challenges values(60292,32473);
insert into challenges values(72974,56685);

select * from challenges;

insert into stats values(47127,26,19);
insert into stats values(47127,15,14);
insert into stats values(18765,43,10);
insert into stats values(18765,72,13);
insert into stats values(75516,35,17);
insert into stats values(60292,11,10);
insert into stats values(72974,41,15);
insert into stats values(75516,75,11);

select * from stats;

insert into submission_stats values(75516,34,12);
insert into submission_stats values(47127,27,10);
insert into submission_stats values(47127,56,18);
insert into submission_stats values(75516,74,12);
insert into submission_stats values(75516,83,8);
insert into submission_stats values(72974,68,24);
insert into submission_stats values(72974,82,14);
insert into submission_stats values(47127,28,11);

select * from submission_stats;
select c.contest_id, c.hacker_id, c.name, sum(total_submissions), sum(total_accepted_submissions),
sum(total_views), sum(total_unique_views)
from contests c
join colleges cg on c.contest_id=cg.contest_id
join challenges ch on cg.college_id=ch.college_id
left join
(
select challenge_id ,sum(total_submissions) as total_submissions,
sum(total_accepted_submissions) as total_accepted_submissions
from submission_stats group by challenge_id
) vw on ch.challenge_id = vw.challenge_id
left join
(
select challenge_id , sum(total_views) as total_views,
sum(total_unique_views) as total_unique_views from

stats group by challenge_id
) ss on ch.challenge_id = ss.challenge_id

group by c.contest_id, c.hacker_id, c.name
having sum(total_submissions)>=0 or
sum(total_accepted_submissions)>=0 or
sum(total_views)>=0 or
sum(total_unique_views)>=0
order by contest_id;

