# Company-Database-Design-in-SQL
This project is performing database designing for an organization in SQL, "The COMPANY" that has offices in several locations. This design will help the company to maintain and keep track of their employees, departments, projects (In-house and Outsource), cost and revenue of the project. 

## ER diagram 
<img width="527" alt="er" src="https://user-images.githubusercontent.com/122247029/226240592-90835ede-0f73-4f47-804c-498fa53ef3fd.PNG">

## Referential Integrity Constraints
<img width="572" alt="reffff" src="https://user-images.githubusercontent.com/122247029/226240638-598b1aef-f5b0-408f-b584-da628e6ac777.PNG">

**Database Creation**
```
CREATE TABLE PROJECT 
(
proj_id INT NOT NULL, 
proj_name VARCHAR(64) NOT NULL, 
proj_type VARCHAR(1) NOT NULL, 
CONSTRAINT PROJECT_PK PRIMARY KEY (proj_id)
);
CREATE TABLE OUTSOURCE_PROJECT 
(
os_proj_id INT NOT NULL, 
os_proj_vendor VARCHAR(64),
os_contract_amount INT, 
CONSTRAINT OUTSOURCE_PROJECT_PK PRIMARY KEY (os_proj_id), 
CONSTRAINT OUTSOURCE_PROJECT_FK1 FOREIGN KEY (os_proj_id) REFERENCES PROJECT(proj_id)
);
CREATE TABLE DEPARTMENT 
(
dept_id INT NOT NULL, 
dept_name VARCHAR(20) NOT NULL,
dept_mgr_id INT NOT NULL, 
dept_mgr_start_date DATE NOT NULL, 
CONSTRAINT DEPARTMENT_PK PRIMARY KEY (dept_id)
);
CREATE TABLE EMPLOYEE
(
emp_id INT NOT NULL, 
emp_ssn VARCHAR(11) NOT NULL,
emp_name VARCHAR(64) NOT NULL, 
emp_sex VARCHAR(1) NOT NULL, 
emp_dob DATE NOT NULL, 
emp_address VARCHAR (64), 
emp_dept_id INT NOT NULL, 
emp_supervisor_id INT, 
emp_salary INT, 
CONSTRAINT EMPLOYEE_PK PRIMARY KEY (emp_id), 
CONSTRAINT EMPLOYEE_FK1 FOREIGN KEY (emp_dept_id) REFERENCES DEPARTMENT(dept_id), 
CONSTRAINT EMPLOYEE_FK2 FOREIGN KEY (emp_supervisor_id) REFERENCES EMPLOYEE(emp_id)
);
CREATE TABLE DEPENDENTS 
(
dep_emp_id INT NOT NULL, 
dep_name VARCHAR(64) NOT NULL, 
dep_sex VARCHAR(1) NOT NULL, 
dep_dob DATE NOT NULL, 
dep_relationship VARCHAR(32) NOT NULL, 
CONSTRAINT DEPENDENTS_PK PRIMARY KEY (dep_emp_id, dep_name), 
CONSTRAINT DEPENDENTS_FK1 FOREIGN KEY (dep_emp_id) REFERENCES EMPLOYEE(emp_id)
);
CREATE TABLE IN_HOUSE_PROJECT 
(
ih_proj_id INT NOT NULL, 
ih_dept_id INT NOT NULL, 
ih_proj_cost INT NOT NULL, 
ih_proj_revenue INT NOT NULL, 
CONSTRAINT IN_HOUSE_PROJECT_PK PRIMARY KEY (ih_proj_id), 
CONSTRAINT IN_HOUSE_PROJECT_FK1 FOREIGN KEY (ih_proj_id) REFERENCES PROJECT(proj_id), 
CONSTRAINT IN_HOUSE_PROJECT_FK2 FOREIGN KEY (ih_dept_id) REFERENCES DEPARTMENT(dept_id)
);
CREATE TABLE EMPLOYEE_PROJECT_ASSIGNMENT
(
ep_emp_id INT NOT NULL, 
ep_proj_id INT NOT NULL, 
ep_hours INT NOT NULL, 
CONSTRAINT EMP_PRJ_ASSIGNMENT_PK PRIMARY KEY (ep_emp_id, ep_proj_id), 
CONSTRAINT EMP_PRJ_ASSIGNMENT_FK1 FOREIGN KEY (ep_emp_id) REFERENCES EMPLOYEE(emp_id), 
CONSTRAINT EMP_PRJ_ASSIGNMENT_FK2 FOREIGN KEY (ep_proj_id) REFERENCES IN_HOUSE_PROJECT(ih_proj_id)
);
CREATE TABLE DEPARTMENT_LOCATION 
(
dept_id INT NOT NULL, 
dept_location VARCHAR(32)	NOT NULL, 
CONSTRAINT DEPARTMENT_LOCATION_PK PRIMARY KEY (dept_id, dept_location), 
CONSTRAINT DEPARTMENT_LOCATION_FK1 FOREIGN KEY (dept_id) REFERENCES DEPARTMENT(dept_id)
);
```

**Inserting values to the database**
```
Insert values to project table
INSERT INTO PROJECT (proj_id, proj_name, proj_type) VALUES (231, 'AWS Deployment', 'O'); 
INSERT INTO PROJECT (proj_id, proj_name, proj_type) VALUES (345, 'Infrastructure setup', 'I'); 
INSERT INTO PROJECT (proj_id, proj_name, proj_type) VALUES (346, 'Java Coding', 'I');
INSERT INTO PROJECT (proj_id, proj_name, proj_type) VALUES (560, 'Customer Complaints', 'O'); 
INSERT INTO PROJECT (proj_id, proj_name, proj_type) VALUES (566, 'XML', 'I');
INSERT INTO PROJECT (proj_id, proj_name, proj_type) VALUES (567, 'JSON', 'I'); 
INSERT INTO PROJECT (proj_id, proj_name, proj_type) VALUES (675, 'ASP', 'O');
INSERT INTO PROJECT (proj_id, proj_name, proj_type) VALUES (765, 'HTML Coding', 'I'); 
INSERT INTO PROJECT (proj_id, proj_name, proj_type) VALUES (787, 'Customer Help Desk', 'O'); 
INSERT INTO PROJECT (proj_id, proj_name, proj_type) VALUES (890, 'Employee Help Desk', 'I');

Insert values to outsource project table
INSERT INTO OUTSOURCE_PROJECT (os_proj_id, os_proj_vendor, os_contract_amount) VALUES (231, 'TCS', 60000);
INSERT INTO OUTSOURCE_PROJECT (os_proj_id, os_proj_vendor, os_contract_amount) VALUES (345, 'A.S Associates', 75000);
INSERT INTO OUTSOURCE_PROJECT (os_proj_id, os_proj_vendor, os_contract_amount) VALUES (560, 'VM Ware', 90000);
INSERT INTO OUTSOURCE_PROJECT (os_proj_id, os_proj_vendor, os_contract_amount) VALUES (787, 'NPW', 120000);
INSERT INTO OUTSOURCE_PROJECT (os_proj_id, os_proj_vendor, os_contract_amount) VALUES (890, 'ASN', 130000);

Insert Values to department table
INSERT INTO DEPARTMENT (dept_id, dept_name, dept_mgr_id, dept_mgr_start_date) VALUES (1, 'Sales', 1, DATE '2017-03-04');
INSERT INTO DEPARTMENT (dept_id, dept_name, dept_mgr_id, dept_mgr_start_date) VALUES (2, 'Marketing', 5, DATE '2015-05-06');
INSERT INTO DEPARTMENT (dept_id, dept_name, dept_mgr_id, dept_mgr_start_date) VALUES (3, 'IT', 4, DATE '2020-06-07');
INSERT INTO DEPARTMENT (dept_id, dept_name, dept_mgr_id, dept_mgr_start_date) VALUES (4, 'Finance', 5, DATE '2019-06-08');
INSERT INTO DEPARTMENT (dept_id, dept_name, dept_mgr_id, dept_mgr_start_date) VALUES (5, 'Operations', 6, DATE '2021-09-08');

Insert values to employee table
INSERT INTO EMPLOYEE (emp_id, emp_ssn, emp_name, emp_sex, emp_dob, emp_address, emp_dept_id, emp_supervisor_id, emp_salary) VALUES (6, '565-09-4569', 'Shelly Gold', 'F', DATE '1991-03-06', '120 West ELM Street 75011 TX', 4, NULL, 160000);
INSERT INTO EMPLOYEE (emp_id, emp_ssn, emp_name, emp_sex, emp_dob, emp_address, emp_dept_id, emp_supervisor_id, emp_salary) VALUES (5, '546-98-0345', 'Matt Johnson', 'M', DATE '1994-09-05', '540 First Street 90035 LA', 1, NULL, 96000);
INSERT INTO EMPLOYEE (emp_id, emp_ssn, emp_name, emp_sex, emp_dob, emp_address, emp_dept_id, emp_supervisor_id, emp_salary) VALUES (1, '232-34-3434', 'Avik Prasad', 'M', DATE '1986-02-05', '122 N Broadway 90012 LA', 4, 6, 140000);
INSERT INTO EMPLOYEE (emp_id, emp_ssn, emp_name, emp_sex, emp_dob, emp_address, emp_dept_id, emp_supervisor_id, emp_salary) VALUES (4, '979-78-0954', 'Mark Smith', 'M', DATE '1972-09-10', '400 fifth avenue 10005 NY', 2, 1, 90000);
INSERT INTO EMPLOYEE (emp_id, emp_ssn, emp_name, emp_sex, emp_dob, emp_address, emp_dept_id, emp_supervisor_id, emp_salary) VALUES (2, '343-65-4567', 'Neena Gupta', 'F', DATE '1990-04-05', '23 S Figueroa Street 90017 LA', 3, 4, 60000);
INSERT INTO EMPLOYEE (emp_id, emp_ssn, emp_name, emp_sex, emp_dob, emp_address, emp_dept_id, emp_supervisor_id, emp_salary) VALUES (3, '124-65-4567', 'Hayden Maxwell', 'F', DATE '1985-05-09', '250 Mills Street 75001 TX', 5, 5, 120000);

Insert values to dependent table
INSERT INTO DEPENDENTS (dep_emp_id, dep_name, dep_sex, dep_dob, dep_relationship) VALUES (1, 'Sean Lopez', 'M', DATE '1991-10-23', 'Brother');
INSERT INTO DEPENDENTS (dep_emp_id, dep_name, dep_sex, dep_dob, dep_relationship) VALUES (1, 'Renne Leach', 'F', DATE '1961-01-10', 'Mother');
INSERT INTO DEPENDENTS (dep_emp_id, dep_name, dep_sex, dep_dob, dep_relationship) VALUES (2, 'Navya Tatman', 'F', DATE '2001-04-06', 'Daughter');
INSERT INTO DEPENDENTS (dep_emp_id, dep_name, dep_sex, dep_dob, dep_relationship) VALUES (2, 'Caroline Thom', 'F', DATE '2003-01-24', 'Daughter');
INSERT INTO DEPENDENTS (dep_emp_id, dep_name, dep_sex, dep_dob, dep_relationship) VALUES (3, 'Taylor Adams', 'F', DATE '1955-04-05', 'Mother');
INSERT INTO DEPENDENTS (dep_emp_id, dep_name, dep_sex, dep_dob, dep_relationship) VALUES (4, 'Ray Madhav', 'F', DATE '1960-02-04', 'Mother');
INSERT INTO DEPENDENTS (dep_emp_id, dep_name, dep_sex, dep_dob, dep_relationship) VALUES (4, 'Carol Jean', 'F', DATE '1987-02-10', 'Spouse');
INSERT INTO DEPENDENTS (dep_emp_id, dep_name, dep_sex, dep_dob, dep_relationship) VALUES (5, 'Krish Sharma', 'M', DATE '1980-04-06', 'Father');
INSERT INTO DEPENDENTS (dep_emp_id, dep_name, dep_sex, dep_dob, dep_relationship) VALUES (6, 'Nancy Campbell', 'F', DATE '1987-07-08', 'Sister');

Insert values to inhouse projec table
INSERT INTO IN_HOUSE_PROJECT (ih_proj_id, ih_dept_id, ih_proj_cost, ih_proj_revenue) VALUES (346, 1, 34000, 56000);
INSERT INTO IN_HOUSE_PROJECT (ih_proj_id, ih_dept_id, ih_proj_cost, ih_proj_revenue) VALUES (566, 2, 90000, 120000);
INSERT INTO IN_HOUSE_PROJECT (ih_proj_id, ih_dept_id, ih_proj_cost, ih_proj_revenue) VALUES (567, 3, 50000, 70000);
INSERT INTO IN_HOUSE_PROJECT (ih_proj_id, ih_dept_id, ih_proj_cost, ih_proj_revenue) VALUES (675, 4, 60000, 120000);
INSERT INTO IN_HOUSE_PROJECT (ih_proj_id, ih_dept_id, ih_proj_cost, ih_proj_revenue) VALUES (765, 5, 300000, 350000);

Insert values to employee project assignment table
INSERT INTO EMPLOYEE_PROJECT_ASSIGNMENT (ep_emp_id, ep_proj_id, ep_hours) VALUES (1, 346, 20);
INSERT INTO EMPLOYEE_PROJECT_ASSIGNMENT (ep_emp_id, ep_proj_id, ep_hours) VALUES (1, 566, 20);
INSERT INTO EMPLOYEE_PROJECT_ASSIGNMENT (ep_emp_id, ep_proj_id, ep_hours) VALUES (2, 567, 10);
INSERT INTO EMPLOYEE_PROJECT_ASSIGNMENT (ep_emp_id, ep_proj_id, ep_hours) VALUES (2, 566, 30);
INSERT INTO EMPLOYEE_PROJECT_ASSIGNMENT (ep_emp_id, ep_proj_id, ep_hours) VALUES (3, 675, 15);
INSERT INTO EMPLOYEE_PROJECT_ASSIGNMENT (ep_emp_id, ep_proj_id, ep_hours) VALUES (3, 346, 25);
INSERT INTO EMPLOYEE_PROJECT_ASSIGNMENT (ep_emp_id, ep_proj_id, ep_hours) VALUES (4, 765, 40);
INSERT INTO EMPLOYEE_PROJECT_ASSIGNMENT (ep_emp_id, ep_proj_id, ep_hours) VALUES (5, 765, 20);
INSERT INTO EMPLOYEE_PROJECT_ASSIGNMENT (ep_emp_id, ep_proj_id, ep_hours) VALUES (5, 675, 20);
INSERT INTO EMPLOYEE_PROJECT_ASSIGNMENT (ep_emp_id, ep_proj_id, ep_hours) VALUES (6, 567, 30);
INSERT INTO EMPLOYEE_PROJECT_ASSIGNMENT (ep_emp_id, ep_proj_id, ep_hours) VALUES (6, 346, 10);

Insert values to department location
INSERT INTO DEPARTMENT_LOCATION (dept_id, dept_location) VALUES (1, 'Los Angeles');
INSERT INTO DEPARTMENT_LOCATION (dept_id, dept_location) VALUES (1, 'Texas'); 
INSERT INTO DEPARTMENT_LOCATION (dept_id, dept_location) VALUES (2, 'Los Angeles'); 
INSERT INTO DEPARTMENT_LOCATION (dept_id, dept_location) VALUES (3, 'New York'); 
INSERT INTO DEPARTMENT_LOCATION (dept_id, dept_location) VALUES (4, 'Texas'); 
INSERT INTO DEPARTMENT_LOCATION (dept_id, dept_location) VALUES (5, 'New York'); 
INSERT INTO DEPARTMENT_LOCATION (dept_id, dept_location) VALUES (5, 'Los Angeles');
```
