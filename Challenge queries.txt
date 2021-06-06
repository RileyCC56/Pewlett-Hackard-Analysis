-- Challenge steps below

-- retrieve the following for retirement table
SELECT e.emp_no,
e.first_name,
e.last_name,
t.title,
t.from_date,
t.to_date
INTO retirement_titles
FROM employees as e
INNER JOIN titles AS t
ON (e.emp_no = t.emp_no)
WHERE (e.birth_date BETWEEN '1952-01-01' AND '1955-12-31')
ORDER BY emp_no ASC;

select * from retirement_titles

-- Use Dictinct with Orderby to remove duplicate rows
SELECT DISTINCT ON (retirement_titles.emp_no)retirement_titles.emp_no,
retirement_titles.first_name,
retirement_titles.last_name,
retirement_titles.title

INTO unique_titles
FROM retirement_titles
ORDER BY emp_no ASC, to_date DESC;

--check the unique titles output
select * from unique_titles
 
-- collect the number of unique work titles from unique titles table then export table
select count(title),title
into retiring_titles
FROM unique_titles
GROUP BY title 
ORDER BY COUNT(title)DESC;

--Part 2 of challenge

--create a mentorship-eligibility table that holds the current employees who were 
--born between January 1, 1965 and December 31, 1965.

--fix needed for the PK and FK for dept_emp table
drop table dept_emp

create table dept_emp (
	emp_no int not null,
	dept_no varchar not null,
	from_date date not null,
	to_date date not null,
	foreign key (emp_no) references employees (emp_no),
	foreign key (dept_no) references departments (dept_no),
	primary key (emp_no, dept_no)
);

--create a mentorship eligibilty table based on employees birth date and if currently employed. 
--join the employee table with titles table PK
--join the dept employee table with employees table PK
-- order the table by employees emp_no
Select distinct on(e.emp_no) e.emp_no,
e.first_name,
e.last_name,
e.birth_date,
de.from_date,
t.title
INTO mentorship_eligibilty
from employees as e
INNER JOIN titles as t
ON e.emp_no = t.emp_no
INNER JOIN dept_emp as de
ON e.emp_no = de.emp_no 
WHERE e.birth_date between '1965-01-01' and '1965-12-31'
and de.to_date = ('9999-01-01')  
Order BY e.emp_no
