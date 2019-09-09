Dunder Mifflien DATABASE
```sql
CREATE TABLE employee ( 
emp_id INT PRIMARY KEY, 
first_name VARCHAR(40), 
last_name VARCHAR(40), 
birth_day DATE, 
sex VARCHAR(1), 
salary INT, 
super_id INT, 
branch_id INT
); 

CREATE TABLE branch ( 
branch_id INT PRIMARY KEY, 
branch_name VARCHAR(40), 
mgr_id INT, 
mgr_start_date DATE, 
FOREIGN KEY(mgr_id) REFERENCES employee(emp_id) ON DELETE SET NULL
); 
 
ALTER TABLE employee 
ADD FOREIGN KEY(branch_id)
REFERENCES branch(branch_id)
ON DELETE SET NULL;

ALTER TABLE employee 
ADD FOREIGN KEY(super_id)
REFERENCES employee(emp_id)
ON DELETE SET NULL;

CREATE TABLE client ( 
client_id INT PRIMARY KEY, 
client_name VARCHAR(40), 
branch_id INT, 
FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE SET NULL 
);  

CREATE TABLE works_with( 
emp_id INT, 
client_id INT, 
total_sales INT, 
PRIMARY KEY(emp_id, client_id), 
FOREIGN KEY(emp_id) REFERENCES employee(emp_id) ON DELETE CASCADE,
FOREIGN KEY(client_id) REFERENCES client(client_id) ON DELETE CASCADE
); 

CREATE TABLE branch_supplier(
branch_id INT, 
supplier_name VARCHAR(40), 
supply_type VARCHAR(40), 
PRIMARY KEY(branch_id, supplier_name), 
FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE CASCADE 
);



---Corporate---
INSERT INTO employee VALUES(100, 'David', 'Wallace', '1967-11-17', 'M', 250000, NULL, NULL);
INSERT INTO branch VALUES(1, 'Corporate', 100, '2006-02-09'); 

UPDATE employee 
SET branch_id = 1 
WHERE emp_id = 100; 

INSERT INTO employee VALUES(101, 'Jan', 'Levinson', '1961-05-11', 'F', 110000, 100, 1); 

---Scranton---
INSERT INTO employee VALUES(102, 'Michael', 'Scott', '1964-03-15' , 'M', 75000, NULL, NULL); 
INSERT INTO branch VALUES(2, 'Scranton', 102, '1992-04-06');

UPDATE employee 
SET branch_id = 2
WHERE emp_id = 102; 

INSERT INTO employee VALUES(103, 'Angela', 'Martin', '1971-06-25','F', 63000, 102, 2);
INSERT INTO employee VALUES(104, 'Kelly', 'Kapoor', '1980-02-05', 'F', 55000, 102, 2);
INSERT INTO employee VALUES(105, 'Stanley', 'Hudson', '1958-02-19', 'M', 69000, 102, 2); 
 
---Stamford---
INSERT INTO employee VALUES(106, 'Josh', 'Porter', '1969-09-05', 'M', 78000, NULL, NULL); 
INSERT INTO branch VALUES (3, 'Stamford', 106, '1998-02-13'); 

UPDATE employee 
SET branch_id = 3 
WHERE emp_id = 106; 

INSERT INTO employee VALUES(107, 'Andy', 'Bernard', '1973-07-22', 'M', '65000', 106, 3); 
INSERT INTO employee VALUES(108, 'Jim', 'Halpert', '1978-10-01', 'M', '71000', 106, 3); 

UPDATE employee  
FOR name = 'Scott' 
SET super_id = 100; 

UPDATE employee
SET super_id = 100
WHERE emp_id = 106;

---BRANCH SUPPLIER
INSERT INTO branch_supplier VALUES(2, 'Hammer Mill', 'Paper');
INSERT INTO branch_supplier VALUES(2, 'Uni-Ball', 'Writing Utensils');
INSERT INTO branch_supplier VALUES(3, 'Patriot Paper' , 'Paper');
INSERT INTO branch_supplier VALUES(2, 'J.T. Forms & Labels', 'Custom Forms');
INSERT INTO branch_supplier VALUES(3, 'Uni-Ball', 'Writing Utensils');
INSERT INTO branch_supplier VALUES(3, 'Hammer Mill', 'Paper');
INSERT INTO branch_supplier VALUES(3, 'Stamford labels', 'Custom Forms');

---CLIENT
INSERT INTO client VALUES(400, 'Dunmore HighSchool', 2); 
INSERT INTO client VALUES(401, 'Lackawana Country', 2); 
INSERT INTO client VALUES(402, 'FedEX', 3); 
INSERT INTO client VALUES(403, 'John Daly Law, LLC', 3);
INSERT INTO client VALUES(404, 'Scranton Whitepages', 2); 
INSERT INTO client VALUES(405, 'Times NewsPaper', 3); 
INSERT INTO client VALUES(406, 'FedEx', 2); 


---WORKS with 
INSERT INTO works_with VALUES(105, 400, 55000); 
INSERT INTO works_with VALUES(102, 401, 267000); 
INSERT INTO works_with VALUES(108, 402, 22500);
INSERT INTO works_with VALUES(107, 403, 5000);
INSERT INTO works_with VALUES(108, 403, 12000);
INSERT INTO works_with VALUES(105, 404, 33000);
INSERT INTO works_with VALUES(107, 405, 26000);
INSERT INTO works_with VALUES(102, 406, 15000);
INSERT INTO works_with VALUES(105, 406, 130000);

SELECT * FROM employee; 
SELECT * FROM branch;
SELECT * FROM client; 
SELECT * FROM works_with; 
SELECT * FROM Branch_supplier; 

--FIND ALL EMPLOYEES ORDERED BY SALARY--

SELECT * FROM employee 
ORDER BY salary DESC; 
 
--- FIND ALL EMPLOYEES ORDER BY SEX THEN NAME --- 

SELECT * FROM employee
ORDER BY sex, first_name, last_name; 

---Find the first 5 employees in the table--- 

SELECT * FROM employee 
LIMIT 5; 

--- FIND THE FIRST AND LAST NAMES OF ALL EMPLOYEES---
SELECT first_name, last_name
FROM employee; 

--- FIND THE FORENAME AND SURNAME OF ALL ATHORITIES --- 
SELECT first_name AS forename, last_name AS surname 
FROM employee;

--- FIND OUT ALL THE DIFFERENT GENDERS --- 
SELECT DISTINCT sex 
FROM employee; 

---SELECT DISTINCT---
SELECT DISTINCT branch_id 
FROM employee; 

---------------------------SQL FUNCTIONS--------------------------------------- 

--FIND THE NUMBER OF EMPLOYEES 

SELECT COUNT(emp_id)
FROM employee; 

SELECT count(super_id)
FROM employee; 

---Find the number of female employees born after 1970 

SELECT COUNT(emp_id)
FROM employee 
WHERE sex = 'F' AND birth_day > '1970-01-01'; 

---Find the Average of ALL Employee Salaries --- 

SELECT AVG(salary) 
FROM employee ; 

---Find the Average Off MAle Employee Salaries 
SELECT AVG(salary)
FROM employee
WHERE sex = 'M';  

--- FIND the Sum of ALL employee's Salaries --- 

SELECT SUM(salary)
FROM employee; 

SELECT SUM(salary)
FROM employee 
WHERE birth_day > '1967-11-17'; 

--- FIND OUT HOW MANY MALES AND HOW MANY FEMALES THERE ARE --- 

SELECT COUNT(sex), sex 
FROM employee 
GROUP BY SEX; 


---FIND THE TOTAL SALES OF EACH SALESMEN 

SELECT SUM(total_sales), emp_id
FROM works_with
GROUP BY emp_id;  


SELECT SUM(total_sales), client_id
FROM works_with
GROUP BY client_id;  


----WILDCARDS---- 
-- % = any # characters, _ = one character 
SELECT * 
FROM client 
WHERE client_name LIKE '%LLC';


--- Find any Branch Suppliers who are in the label business 
SELECT * 
FROM branch_supplier
WHERE supplier_name LIKE '%LABELS'; 

--- Find any Employeee born in October --- 

SELECT * 
FROM employee 
WHERE birth_day LIKE '____-02%'; 


SELECT * FROM client
WHERE client_name LIKE '%school'; 

--- UNION --- 
-- A special SQL operator to combine multiple statements into one --- 

--- Find a list of employee and branch names --- 


SELECT first_name AS COMPANY_NAMES 
FROM employee 
UNION 
SELECT branch_name 
FROM branch 
UNION 
SELECT client_name 
FROM CLIENT; 

--- FIND A LIST OF ALL CLIENTS and branch suppliers' names 

SELECT client_name, client.branch_id 
FROM client 
UNION 
SELECT supplier_name, branch_supplier.branch_id  
FROM branch_supplier; 


-- Find a list of all money spent or earned by the company 

SELECT salary, employee.emp_id 
FROM employee 
UNION 
SELECT total_sales, total_sales.emp_id 
FROM works_with; 


SELECT salary, emp_id 
FROM employee 
UNION 
SELECT total_sales, emp_id 
FROM works_with;

UPDATED DUNDER MIFFLIN DATABASE
CREATE TABLE employee ( 
emp_id INT PRIMARY KEY, 
first_name VARCHAR(40), 
last_name VARCHAR(40), 
birth_day DATE, 
sex VARCHAR(1), 
salary INT, 
super_id INT, 
branch_id INT
); 

CREATE TABLE branch ( 
branch_id INT PRIMARY KEY, 
branch_name VARCHAR(40), 
mgr_id INT, 
mgr_start_date DATE, 
FOREIGN KEY(mgr_id) REFERENCES employee(emp_id) ON DELETE SET NULL
); 
 
ALTER TABLE employee 
ADD FOREIGN KEY(branch_id)
REFERENCES branch(branch_id)
ON DELETE SET NULL;

ALTER TABLE employee 
ADD FOREIGN KEY(super_id)
REFERENCES employee(emp_id)
ON DELETE SET NULL;

CREATE TABLE client ( 
client_id INT PRIMARY KEY, 
client_name VARCHAR(40), 
branch_id INT, 
FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE SET NULL 
);  

CREATE TABLE works_with( 
emp_id INT, 
client_id INT, 
total_sales INT, 
PRIMARY KEY(emp_id, client_id), 
FOREIGN KEY(emp_id) REFERENCES employee(emp_id) ON DELETE CASCADE,
FOREIGN KEY(client_id) REFERENCES client(client_id) ON DELETE CASCADE
); 

CREATE TABLE branch_supplier(
branch_id INT, 
supplier_name VARCHAR(40), 
supply_type VARCHAR(40), 
PRIMARY KEY(branch_id, supplier_name), 
FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE CASCADE 
);

---Corporate---
INSERT INTO employee VALUES(100, 'David', 'Wallace', '1967-11-17', 'M', 250000, NULL, NULL);
INSERT INTO branch VALUES(1, 'Corporate', 100, '2006-02-09'); 

UPDATE employee 
SET branch_id = 1 
WHERE emp_id = 100; 

INSERT INTO employee VALUES(101, 'Jan', 'Levinson', '1961-05-11', 'F', 110000, 100, 1); 

---Scranton---
INSERT INTO employee VALUES(102, 'Michael', 'Scott', '1964-03-15' , 'M', 75000, NULL, NULL); 
INSERT INTO branch VALUES(2, 'Scranton', 102, '1992-04-06');

UPDATE employee 
SET branch_id = 2
WHERE emp_id = 102; 

INSERT INTO employee VALUES(103, 'Angela', 'Martin', '1971-06-25','F', 63000, 102, 2);
INSERT INTO employee VALUES(104, 'Kelly', 'Kapoor', '1980-02-05', 'F', 55000, 102, 2);
INSERT INTO employee VALUES(105, 'Stanley', 'Hudson', '1958-02-19', 'M', 69000, 102, 2); 
 
---Stamford---
INSERT INTO employee VALUES(106, 'Josh', 'Porter', '1969-09-05', 'M', 78000, NULL, NULL); 
INSERT INTO branch VALUES (3, 'Stamford', 106, '1998-02-13'); 

UPDATE employee 
SET branch_id = 3 
WHERE emp_id = 106; 

INSERT INTO employee VALUES(107, 'Andy', 'Bernard', '1973-07-22', 'M', '65000', 106, 3); 
INSERT INTO employee VALUES(108, 'Jim', 'Halpert', '1978-10-01', 'M', '71000', 106, 3); 

UPDATE employee  
FOR name = 'Scott' 
SET super_id = 100; 

UPDATE employee
SET super_id = 100
WHERE emp_id = 106;

---BRANCH SUPPLIER
INSERT INTO branch_supplier VALUES(2, 'Hammer Mill', 'Paper');
INSERT INTO branch_supplier VALUES(2, 'Uni-Ball', 'Writing Utensils');
INSERT INTO branch_supplier VALUES(3, 'Patriot Paper' , 'Paper');
INSERT INTO branch_supplier VALUES(2, 'J.T. Forms & Labels', 'Custom Forms');
INSERT INTO branch_supplier VALUES(3, 'Uni-Ball', 'Writing Utensils');
INSERT INTO branch_supplier VALUES(3, 'Hammer Mill', 'Paper');
INSERT INTO branch_supplier VALUES(3, 'Stamford labels', 'Custom Forms');

---CLIENT
INSERT INTO client VALUES(400, 'Dunmore HighSchool', 2); 
INSERT INTO client VALUES(401, 'Lackawana Country', 2); 
INSERT INTO client VALUES(402, 'FedEX', 3); 
INSERT INTO client VALUES(403, 'John Daly Law, LLC', 3);
INSERT INTO client VALUES(404, 'Scranton Whitepages', 2); 
INSERT INTO client VALUES(405, 'Times NewsPaper', 3); 
INSERT INTO client VALUES(406, 'FedEx', 2); 


---WORKS with 
INSERT INTO works_with VALUES(105, 400, 55000); 
INSERT INTO works_with VALUES(102, 401, 267000); 
INSERT INTO works_with VALUES(108, 402, 22500);
INSERT INTO works_with VALUES(107, 403, 5000);
INSERT INTO works_with VALUES(108, 403, 12000);
INSERT INTO works_with VALUES(105, 404, 33000);
INSERT INTO works_with VALUES(107, 405, 26000);
INSERT INTO works_with VALUES(102, 406, 15000);
INSERT INTO works_with VALUES(105, 406, 130000);

SELECT * FROM employee; 
SELECT * FROM branch;
SELECT * FROM client; 
SELECT * FROM works_with; 
SELECT * FROM Branch_supplier; 

--FIND ALL EMPLOYEES ORDERED BY SALARY--

SELECT * FROM employee 
ORDER BY salary DESC; 
 
--- FIND ALL EMPLOYEES ORDER BY SEX THEN NAME --- 

SELECT * FROM employee
ORDER BY sex, first_name, last_name; 

---Find the first 5 employees in the table--- 

SELECT * FROM employee 
LIMIT 5; 

--- FIND THE FIRST AND LAST NAMES OF ALL EMPLOYEES---
SELECT first_name, last_name
FROM employee; 

--- FIND THE FORENAME AND SURNAME OF ALL ATHORITIES --- 
SELECT first_name AS forename, last_name AS surname 
FROM employee;

--- FIND OUT ALL THE DIFFERENT GENDERS --- 
SELECT DISTINCT sex 
FROM employee; 

---SELECT DISTINCT---
SELECT DISTINCT branch_id 
FROM employee; 

---------------------------SQL FUNCTIONS--------------------------------------- 

--FIND THE NUMBER OF EMPLOYEES 

SELECT COUNT(emp_id)
FROM employee; 

SELECT count(super_id)
FROM employee; 

---Find the number of female employees born after 1970 

SELECT COUNT(emp_id)
FROM employee 
WHERE sex = 'F' AND birth_day > '1970-01-01'; 

---Find the Average of ALL Employee Salaries --- 

SELECT AVG(salary) 
FROM employee ; 

---Find the Average Off MAle Employee Salaries 
SELECT AVG(salary)
FROM employee
WHERE sex = 'M';  

--- FIND the Sum of ALL employee's Salaries --- 

SELECT SUM(salary)
FROM employee; 

SELECT SUM(salary)
FROM employee 
WHERE birth_day > '1967-11-17'; 

--- FIND OUT HOW MANY MALES AND HOW MANY FEMALES THERE ARE --- 

SELECT COUNT(sex), sex 
FROM employee 
GROUP BY SEX; 


---FIND THE TOTAL SALES OF EACH SALESMEN 

SELECT SUM(total_sales), emp_id
FROM works_with
GROUP BY emp_id;  


SELECT SUM(total_sales), client_id
FROM works_with
GROUP BY client_id;  


----WILDCARDS---- 
-- % = any # characters, _ = one character 
SELECT * 
FROM client 
WHERE client_name LIKE '%LLC';


--- Find any Branch Suppliers who are in the label business 
SELECT * 
FROM branch_supplier
WHERE supplier_name LIKE '%LABELS'; 

--- Find any Employeee born in October --- 

SELECT * 
FROM employee 
WHERE birth_day LIKE '____-02%'; 


SELECT * FROM client
WHERE client_name LIKE '%school'; 

--- UNION --- 
-- A special SQL operator to combine multiple statements into one --- 

--- Find a list of employee and branch names --- 


SELECT first_name AS COMPANY_NAMES 
FROM employee 
UNION 
SELECT branch_name 
FROM branch 
UNION 
SELECT client_name 
FROM CLIENT; 

--- FIND A LIST OF ALL CLIENTS and branch suppliers' names 

SELECT client_name, client.branch_id 
FROM client 
UNION 
SELECT supplier_name, branch_supplier.branch_id  
FROM branch_supplier; 


-- Find a list of all money spent or earned by the company 

SELECT salary, employee.emp_id 
FROM employee 
UNION 
SELECT total_sales, total_sales.emp_id 
FROM works_with; 


SELECT salary, emp_id 
FROM employee 
UNION 
SELECT total_sales, emp_id 
FROM works_with; 

SELECT * FROM employee; 

DESCRIBE client_master; 


CREATE TABLE client_master(
name VARCHAR(20),
Address VARCHAR(20),
phone_number INT,
fax_number INT,
balance_number INT
    );
    

CREATE TABLE client_namfsdfs
POSTGRESS SERVER
CREATE TABLE client_master(
name VARCHAR(20),
Address VARCHAR(20),
telephone_number INT,
fax_number INT,
balance_number INT
    );

DROP table client_master;

SELECT * FROM client_master;

SELECT * FROM INFORMATION_SCHEMA.COLUMNS where table_name = 'client_master';

CREATE TABLE product_master(
    column_name VARCHAR(20),
    DATA TYPE
 );

CREATE TABLE web_master
(
    client_no VARCHAR(6),
    name      VARCHAR(20),
    address1  VARCHAR(30),
    address2  VARCHAR(30),
    city      VARCHAR(15),
    state     VARCHAR(15),
    pin       INT,
    remarks   VARCHAR(60),
    bal_due   INT
);

INSERT INTO web_master VALUES (C0200, 'Probjakar Rane', 'A-5 Jay Apartments',
'Service Road, Vile Parke', 'Bombay','Maharashtra', 400057
);


CREATE TABLE employee ( 
emp_id INT PRIMARY KEY, 
first_name VARCHAR(40), 
last_name VARCHAR(40), 
birth_day DATE, 
sex VARCHAR(1), 
salary INT, 
super_id INT, 
branch_id INT
); 

CREATE TABLE branch ( 
branch_id INT PRIMARY KEY, 
branch_name VARCHAR(40), 
mgr_id INT, 
mgr_start_date DATE, 
FOREIGN KEY(mgr_id) REFERENCES employee(emp_id) ON DELETE SET NULL
); 
 
ALTER TABLE employee 
ADD FOREIGN KEY(branch_id)
REFERENCES branch(branch_id)
ON DELETE SET NULL;

ALTER TABLE employee 
ADD FOREIGN KEY(super_id)
REFERENCES employee(emp_id)
ON DELETE SET NULL;

CREATE TABLE client ( 
client_id INT PRIMARY KEY, 
client_name VARCHAR(40), 
branch_id INT, 
FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE SET NULL 
);  

CREATE TABLE works_with( 
emp_id INT, 
client_id INT, 
total_sales INT, 
PRIMARY KEY(emp_id, client_id), 
FOREIGN KEY(emp_id) REFERENCES employee(emp_id) ON DELETE CASCADE,
FOREIGN KEY(client_id) REFERENCES client(client_id) ON DELETE CASCADE
); 

CREATE TABLE branch_supplier(
branch_id INT, 
supplier_name VARCHAR(40), 
supply_type VARCHAR(40), 
PRIMARY KEY(branch_id, supplier_name), 
FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE CASCADE 
);


INSERT INTO employee VALUES(100, 'David', 'Wallace', '1967-11-17', 'M', 250000, NULL, NULL);
INSERT INTO branch VALUES(1, 'Corporate', 100, '2006-02-09'); 

UPDATE employee 
SET branch_id = 1 
WHERE emp_id = 100; 

INSERT INTO employee VALUES(101, 'Jan', 'Levinson', '1961-05-11', 'F', 110000, 100, 1); 

SELECT * FROM employee; 
SELECT * FROM branch;

INSERT INTO employee VALUES(102, 'Michael', 'Scott', '1964-03-15' , 'M', 75000, NULL, NULL); 
INSERT INTO branch VALUES(2, 'Scranton', 102, '1992-04-06');

UPDATE employee 
SET branch_id = 2
WHERE emp_id = 102; 

INSERT INTO employee VALUES(103, 'Angela', 'Martin', '1971-06-25','F', 63000, 102, 2);
INSERT INTO employee VALUES(104, 'Kelly', 'Kapoor', '1980-02-05', 'F', 55000, 102, 2);
INSERT INTO employee VALUES(105, 'Stanley', 'Hudson', '1958-02-19', 'M', 69000, 102, 2); 

INSERT INTO employee VALUES(106, 'Josh', 'Porter', '1969-09-05', 'M', 78000, NULL, NULL); 
INSERT INTO branch VALUES (3, 'Stamford', 106, '1998-02-13'); 


UPDATE employee 
SET branch_id = 3 
WHERE emp_id = 106; 

INSERT INTO employee VALUES(107, 'Andy', 'Bernard', '1973-07-22', 'M', '65000', 106, 3); 
INSERT INTO employee VALUES(107, 'Andy', 'Bernard', '1973-07-22', 'M', '65000', 106, 3); 
INSERT INTO employee VALUES(108, 'Jim', 'Halpert', '1978-10-01', 'M', '71000', 106, 3); 

UPDATE employee  
FOR name = 'Scott' 
SET super_id = 100; 

UPDATE employee
SET super_id = 100
WHERE emp_id = 106;


CREATE TABLE employee ( 
emp_id INT PRIMARY KEY, 
first_name VARCHAR(40), 
last_name VARCHAR(40), 
birth_day DATE, 
sex VARCHAR(1), 
salary INT, 
super_id INT, 
branch_id INT
); 

CREATE TABLE branch ( 
branch_id INT PRIMARY KEY, 
branch_name VARCHAR(40), 
mgr_id INT, 
mgr_start_date DATE, 
FOREIGN KEY(mgr_id) REFERENCES employee(emp_id) ON DELETE SET NULL
); 
 
ALTER TABLE employee 
ADD FOREIGN KEY(branch_id)
REFERENCES branch(branch_id)
ON DELETE SET NULL;

ALTER TABLE employee 
ADD FOREIGN KEY(super_id)
REFERENCES employee(emp_id)
ON DELETE SET NULL;

CREATE TABLE client ( 
client_id INT PRIMARY KEY, 
client_name VARCHAR(40), 
branch_id INT, 
FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE SET NULL 
);  

CREATE TABLE works_with( 
emp_id INT, 
client_id INT, 
total_sales INT, 
PRIMARY KEY(emp_id, client_id), 
FOREIGN KEY(emp_id) REFERENCES employee(emp_id) ON DELETE CASCADE,
FOREIGN KEY(client_id) REFERENCES client(client_id) ON DELETE CASCADE
); 

CREATE TABLE branch_supplier(
branch_id INT, 
supplier_name VARCHAR(40), 
supply_type VARCHAR(40), 
PRIMARY KEY(branch_id, supplier_name), 
FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE CASCADE 
);

---Corporate---
INSERT INTO employee VALUES(100, 'David', 'Wallace', '1967-11-17', 'M', 250000, NULL, NULL);
INSERT INTO branch VALUES(1, 'Corporate', 100, '2006-02-09'); 

UPDATE employee 
SET branch_id = 1 
WHERE emp_id = 100; 

INSERT INTO employee VALUES(101, 'Jan', 'Levinson', '1961-05-11', 'F', 110000, 100, 1); 

---Scranton---
INSERT INTO employee VALUES(102, 'Michael', 'Scott', '1964-03-15' , 'M', 75000, NULL, NULL); 
INSERT INTO branch VALUES(2, 'Scranton', 102, '1992-04-06');

UPDATE employee 
SET branch_id = 2
WHERE emp_id = 102; 

INSERT INTO employee VALUES(103, 'Angela', 'Martin', '1971-06-25','F', 63000, 102, 2);
INSERT INTO employee VALUES(104, 'Kelly', 'Kapoor', '1980-02-05', 'F', 55000, 102, 2);
INSERT INTO employee VALUES(105, 'Stanley', 'Hudson', '1958-02-19', 'M', 69000, 102, 2); 
 
---Stamford---
INSERT INTO employee VALUES(106, 'Josh', 'Porter', '1969-09-05', 'M', 78000, NULL, NULL); 
INSERT INTO branch VALUES (3, 'Stamford', 106, '1998-02-13'); 

UPDATE employee 
SET branch_id = 3 
WHERE emp_id = 106; 

INSERT INTO employee VALUES(107, 'Andy', 'Bernard', '1973-07-22', 'M', '65000', 106, 3); 
INSERT INTO employee VALUES(108, 'Jim', 'Halpert', '1978-10-01', 'M', '71000', 106, 3); 

UPDATE employee  
FOR name = 'Scott' 
SET super_id = 100; 

UPDATE employee
SET super_id = 100
WHERE emp_id = 106;

---BRANCH SUPPLIER
INSERT INTO branch_supplier VALUES(2, 'Hammer Mill', 'Paper');
INSERT INTO branch_supplier VALUES(2, 'Uni-Ball', 'Writing Utensils');
INSERT INTO branch_supplier VALUES(3, 'Patriot Paper' , 'Paper');
INSERT INTO branch_supplier VALUES(2, 'J.T. Forms & Labels', 'Custom Forms');
INSERT INTO branch_supplier VALUES(3, 'Uni-Ball', 'Writing Utensils');
INSERT INTO branch_supplier VALUES(3, 'Hammer Mill', 'Paper');
INSERT INTO branch_supplier VALUES(3, 'Stamford labels', 'Custom Forms');

---CLIENT
INSERT INTO client VALUES(400, 'Dunmore HighSchool', 2); 
INSERT INTO client VALUES(401, 'Lackawana Country', 2); 
INSERT INTO client VALUES(402, 'FedEX', 3); 
INSERT INTO client VALUES(403, 'John Daly Law, LLC', 3);
INSERT INTO client VALUES(404, 'Scranton Whitepages', 2); 
INSERT INTO client VALUES(405, 'Times NewsPaper', 3); 
INSERT INTO client VALUES(406, 'FedEx', 2); 


---WORKS with 
INSERT INTO works_with VALUES(105, 400, 55000); 
INSERT INTO works_with VALUES(102, 401, 267000); 
INSERT INTO works_with VALUES(108, 402, 22500);
INSERT INTO works_with VALUES(107, 403, 5000);
INSERT INTO works_with VALUES(108, 403, 12000);
INSERT INTO works_with VALUES(105, 404, 33000);
INSERT INTO works_with VALUES(107, 405, 26000);
INSERT INTO works_with VALUES(102, 406, 15000);
INSERT INTO works_with VALUES(105, 406, 130000);

SELECT * FROM employee; 
SELECT * FROM branch;
SELECT * FROM client; 
SELECT * FROM works_with; 
SELECT * FROM Branch_supplier; 

--FIND ALL EMPLOYEES ORDERED BY SALARY--

SELECT * FROM employee 
ORDER BY salary DESC; 
 
--- FIND ALL EMPLOYEES ORDER BY SEX THEN NAME --- 

SELECT * FROM employee
ORDER BY sex, first_name, last_name; 

---Find the first 5 employees in the table--- 

SELECT * FROM employee 
LIMIT 5; 

--- FIND THE FIRST AND LAST NAMES OF ALL EMPLOYEES---
SELECT first_name, last_name
FROM employee; 

--- FIND THE FORENAME AND SURNAME OF ALL ATHORITIES --- 
SELECT first_name AS forename, last_name AS surname 
FROM employee;

--- FIND OUT ALL THE DIFFERENT GENDERS --- 
SELECT DISTINCT sex 
FROM employee; 

---SELECT DISTINCT---
SELECT DISTINCT branch_id 
FROM employee; 

---------------------------SQL FUNCTIONS--------------------------------------- 

--FIND THE NUMBER OF EMPLOYEES 

SELECT COUNT(emp_id)
FROM employee; 

SELECT count(super_id)
FROM employee; 

---Find the number of female employees born after 1970 

SELECT COUNT(emp_id)
FROM employee 
WHERE sex = 'F' AND birth_day > '1970-01-01'; 

---Find the Average of ALL Employee Salaries --- 

SELECT AVG(salary) 
FROM employee ; 

---Find the Average Off MAle Employee Salaries 
SELECT AVG(salary)
FROM employee
WHERE sex = 'M';  

--- FIND the Sum of ALL employee's Salaries --- 

SELECT SUM(salary)
FROM employee; 

SELECT SUM(salary)
FROM employee 
WHERE birth_day > '1967-11-17'; 

--- FIND OUT HOW MANY MALES AND HOW MANY FEMALES THERE ARE --- 

SELECT COUNT(sex), sex 
FROM employee 
GROUP BY SEX; 


---FIND THE TOTAL SALES OF EACH SALESMEN 

SELECT SUM(total_sales), emp_id
FROM works_with
GROUP BY emp_id;  


SELECT SUM(total_sales), client_id
FROM works_with
GROUP BY client_id;  


----WILDCARDS---- 
-- % = any # characters, _ = one character 
SELECT * 
FROM client 
WHERE client_name LIKE '%LLC';


--- Find any Branch Suppliers who are in the label business 
SELECT * 
FROM branch_supplier
WHERE supplier_name LIKE '%LABELS'; 

--- Find any Employeee born in October --- 

SELECT * 
FROM employee 
WHERE birth_day LIKE '____-02%'; 


SELECT * FROM client
WHERE client_name LIKE '%school'; 

--- UNION --- 
-- A special SQL operator to combine multiple statements into one --- 

--- Find a list of employee and branch names --- 


SELECT first_name AS COMPANY_NAMES 
FROM employee 
UNION 
SELECT branch_name 
FROM branch 
UNION 
SELECT client_name 
FROM CLIENT; 

--- FIND A LIST OF ALL CLIENTS and branch suppliers' names 

SELECT client_name, client.branch_id 
FROM client 
UNION 
SELECT supplier_name, branch_supplier.branch_id  
FROM branch_supplier; 


-- Find a list of all money spent or earned by the company 

SELECT salary, employee.emp_id 
FROM employee 
UNION 
SELECT total_sales, total_sales.emp_id 
FROM works_with; 


SELECT salary, emp_id 
FROM employee 
UNION 
SELECT total_sales, emp_id 
FROM works_with; 

SELECT * FROM employee; 

DESCRIBE client_master; 


CREATE TABLE client_master(
name VARCHAR(20),
Address VARCHAR(20),
phone_number INT,
fax_number INT,
balance_number INT
    );
    

CREATE TABLE client_namfsdfs

```