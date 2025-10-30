--EMPLOYEE MANAGEMENT SYSTEM

CREATE TABLE employee(
    emp_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    department VARCHAR(100),
    salary FLOAT
);

--Database Connection
con=mysql.connector.connect(
    host="localhost",
    password="root",
    database="employee_db"
)

cursor=con.cursor()

--Add new employee
def add_employee():
    name=input("enter employee name:")
    department=input(enter department:")
    salary=float(input("enter salary:"))
    query="INSERT INTO employee (name, department, salary)"
            VALUES (%s,%s,%s)
    cursor.execute(query,(name, department, salary))
    con.commit()
    Print("Employee Added Successfully!")

--View All Employee
def view_employee():
    cursor.execute("Select * From employee")
    row= cursor.fetchall()
    print("\n--Employee Records--")
    for row in rows:
    print(f"ID:{row[0]}, name:{[1]}, Dept: {row[2]}, salary: {[3]}")
    Print("------")

--Search Employee
def search_employee():
    emp_id= input("Enter Employee ID To Search:")
    cursor.execute("Select * From Employee WHERE emp_id = %s"'(emp_id,))
    row = cursor.fetchone()
    if row:
    Print(f"\nID: {row[0]], name: {row[1]}, Dept: {row[2]}, salary: {row[3]}")
    else:
    Print("Employee Not Found")


--Update Employee
def update_employee():
    emp_id = input("Enter Employee ID To Update:")
    new_salary = float(input("Enter new salary:"))
    query = "UPDATE employees Set salary = %s WHERE emp_id=%s"
    cursor.execute(query,(new_salary, emp_id))
    con.commit()
    Print("Employee Updated Successfully")


--Delete Employee
def delete_employee():
    emp_id=input("Enter Employee ID To Delete:")
    cursor.execute("DELETE FROM employee WHERE emp_id=%s",(emp_id,))
    con.commit()
    Print("Employee Deleted Successfully")


--Menu
def menu():
  while True:
    print("\n---Employee Management System---")
    Print("1. Add Employee")
    Print("2. View All Employee")
    Print("3. Search Employee by ID")
    Print("4. Update Employee")
    Print("5. Delete Employee")
    Print("6. Exit")
    Choice=input("Enter your choice(1-6):")



if choice = '1':
  add_employee()

if choice = '2':
  view_employee()

if choice = '3':
  search_employee()

if choice = '4':
  update_employee()

if choice = '5':
  delete_employee()

if choice = '6':
  
  Print("Exiting Program")
  Break
else:
  Print("Invalid choice! Please Try Again.")

if_name="main":
  main()
  cursor.close()
  con.close()


-------EMPLOYEE MANAGEMENT SYSTEM-------
1. Add Employee
2. View All Employee
3. Search Employee by ID
4. Update Employee
5. Delete Employee
6. Exit

Enter your choice(1-6): 1
Enter Employee Name: Shashank Mittal 
Enter Department: IT
Enter Salary: 80000
Employee added successfully!

--Drop if exists (safe for development)
DROP TABLE IF EXISTS Intern_Project;
DROP TABLE IF EXISTS Project;
DROP TABLE IF EXISTS Intern;
DROP TABLE IF EXISTS Manager;
DROP TABLE IF EXISTS Depatment;

--Departments
CREATE TABLE Department(
    dept_id Serial PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE,
    location VARCHAR(100)
);

--Managers
CREATE TABLE Manager(
    manager_id SERIAL PRIMARY KEY,
    First_name VARCHAR(50) NOT NULL,
    Last_name VARCHAR(50) NOT NULL,
    email_id VARCHAR(100) UNIQUE,
    dept_id INT REFERANCES Department(dept_id) ON DELETE SET NULL
);

--Projects
CREATE TABLE Project(
    project_id SERIAL PRIMARY KEY,
    title VARHAR(100) NOT NULL,
    description TEXT,
    manager_id INT REFRENCES Manager(Manager_id) ON DELETE SET NULL,
    start_date DATE,
    end_date DATE,
    status VARCHAR(50) DEFAULT 'planning'
);

--Junction Table: mant-to-many Intern<->Project (with role and hours)
CREATE TABLE Intern_project(
    Intern_id INT NOT NULL REFERANCES Intern(Intern_id) ON DELETE CASCADE,
    Project_id INT NOT NULL REFERANCES Project(Project_id) ON DELETE CASCADE,
    assigned_on DATE DEFAULT CURRENT_DATE,
    role VARCHAR(80),
    weekly_hours INT CHECK (weekly_hours>=0),
    PRIMARY KEY (intern_id, Project_id)
);

--Example indexes (optional)
CREATE INDEX idx_intern_dept ON Intern(dept_id);
CREATE INDEX idx_Project_manager ON Project(manger_id);

--Sample Data
INSERT INTO Department (name, location)
    VALUES ('Engineering','Dehradun'),('QA','Noida'),('DevOps','Bengaluru);

INSERT INTO Manager (first_name, last_name, email_id, dept_id)
        VALUES ('Sankalp', 'Srivastava', 'srivastavasankalpos28@gmail.com', 1),
        ('Aishwariya', 'Singh', 'aishwariya@example.com',2);

INSERT INTO Project (title, description, manager_id, start_date, status)
        VALUES ('DB Optimization','Optimize queries and schema', 1, '2025-08-01', ongoing),
        ('Test Automation', 'Create automation test suites', 2,'2025-09-01', ongoing);

INSERT INTO Intern (first_name, last_name, email_id, university, start_date, dept_id)
        VALUES ('Vyom','Singh','officialvyomsingh2001@gmail.com, RKGIT, '2025-08-01',1)
        ('Haseena', 'Rizwi', 'hasrizwi@example.com', 'XYZ',2);

INSERT INTO Intern_Project (intern_id, Project_id, role, weekly_hours)
        VALUES (1,1,'DB Developer', 40),
        (2,2,'QA Engineer', 28);

ERDiagram
Department{
    int dept_id PK
    string name
    string location
}

Manager{
    int Manager_id PK
    string first_name
    string last_name
    string email
    int dept_id FK
}

Project{
    int Project_id PK
    string title
    string description
    int mamager_id FK
    date start_date
    date end_date
    string status
}

Intern{
    int Intern_id PK
    string first_name
    string last_name
    string email
    string university
    date start_date
    date end_date
    int dept_id FK
}

Intern_Project{
    int intern_id FK
    int project_id FK
    date assigned_on
    string role
    int weekly_hours
    PK(intern_id,project_id)
}

Department |--o{ MANAGER: "Has"
Department |--o{ INTERN: "Host"
MANAGER |--o{ PROJECT: "Leads"
INTERN |--o{ INTERN_PROJECT: "Participates"
PROJECT |--o{ INTERN_PROJECT: "Includes"

@startuml
    'Entities
    entity "Department" as Dept{
        *dept_id : int<<PK>>
        --
        name : varchar
        location : varchar
    }

    entity "Manager" as Mgr{
        *manager_id : int<<PK>>
        --
        first_name
        last_name
        email
        dept_id : int<<FK>>
        }

    entity "Project" as Project{
        *project_id : <<PK>>
        __
        title
        description
        manager_id : <<FK>>
        start_date
        end_date
        status
        }

    entity "Intern" as Intern{
        *intern_id : int<<PK>>
        --
        first_name
        last_name
        email
        university
        start_date
        end_date
        dept_id : int<<FK>>
        }

    entity "Intern_Project" as IP{
        *intern_id : <<PK>>
        *project_id : <<PK>>
        --
        assigned_on
        role
        weekly_hours
        }

    'Relationships (cardinality verbal)
    Dept |--o{ Mgr : "Has"
    Dept |--o{ Intern : "Hosts"
    Mgr |--o{ Project : "Leads"
    Intern |--o{ IP : "assigned to"
    Project |--o{ IP : "Has"

@enduml

digraph ER{
    graph [rankdir=LR];
    node [shape=record, frontname="Helvetica"];


Department [label="{Deprtment|+ dept_id : int\|+ name : varchar\|+ location : varchar\|}"];
Manager [label="{Manager|+ manager_id : int\|+ first_name : varchar\|+ last_name : varchar\|+ email : varchar\|+ dept_id : int(FK)\|}"]
Project [label="{Project|+ project_id : int\|+ title : varchar\|+ description : text\|+ manager_id : int(FK)\|+ start_date : date\|+ end_date\|+ status : varchar\|}"]:
Intern [label="{Intern|+ intern_id : int\|+ first_name : varchar\|+ last_name : varchar\|+ email : varchar\|+ university : varchar\|+ start_date : date\|+ end_date : date\|+ dept_id : int (FK)\|}"];
InternProject [label="{Intern_Project|+ intern_id : int (FK)\|+ assigned_on : date\|+ role : varchar\|+ weekly_hours : int\|}"];

Department -> Manager [label="1..*"];
Department -> Intern [label="1..*"];
Manager -> Project [label="1..*"];
Intern -> InternProject [label="1..*"]
Project -> InternProject [label="1..*"];
}

--IT INTERNSHIP PROJECT MANAGEMENT DATABASE
--
DROP TABLE IF EXISTS Intern_Project;
DROP TABLE IF EXISTS Project;
DROP TABLE IF EXISTS Intern;
DROP TABLE IF EXISTS Manager;
DROP TABLE IF EXISTS Department;

--Department Table 
--
CREATE TABLE Department(
    dept_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    dept_name VARCHAR(100) NOT NULL,
    location VARCHAR(100)
);

--Manager Table
--
CREATE TABLE Manager(
    manager_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50),
    email VARCHAR(100) UNIQUE,
    phone VARCHAR(15),
    dept_id INT,
    FOREIGN KEY (dept_id) REFERANCES Department(dept_id)
);

--Intern Table
--
CRETE TABLE Intern(
    intern_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50),
    email VARCHAR(100) UNIQUE,
    university VARCHAR(100)'
    start_date DATE,
    end_date DATE,
    dept_id INT
);

--Project: IT INTERNSHIP MANAGEMENT SYSTEM 
--Description: ER DIAGRAM IMPLIMENTATION IN SQL
--Author: VYOM SINGH
--

--DROP OLD TABLES (Safe re-run)
DROP TABLE IF EXISTS Timesheet;
DROP TABLE IF EXISTS Intern_Project;
DROP TABLE IF EXISTS Project;
DROP TABLE IF EXISTS Intern;
DROP TABLE IF EXISTS Manager;
DROP TABLE IF EXISTS department;

--Department Table
CREATE TABLE Depertment(
    dept_id INT GENERATE ALWAYS AS IDENTITY PRIMARY KEY,
    dept_name VARCHAR(100) NOT NULL UNIQUE,
    location VARCHAR(100) DEFAULT 'DEHRADUN',
    created_on TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

--Manager Table
CREATE TABLE Manager(
    manager_id INT GENERATE ALWAYS AS IDENTITY PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50),
    email VARCHAR(100) UNIQUE,
    dept_id INT,
    hire_date DATE DEFAULT CURRENT_DATE,
    FOREIGN KEY (dept_id) REFERANCES Department(dept_id) ON DELETE SET NULL
);

--Intern Table
CREATE TABLE Intern(
    intern_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50),
    email VARCHAR(100) UNIQUE,
    university VARCHAR(100),
    cgpa DECIMAL(3,2) check (cgpa>=0 AND cgpa<=10),
    start_date DATE,
    end_date DATE,
    dept_id INT,
    FOREIGN KEY (dept_id) REFERANCES Department(dept_id) ON DELETE SET NULL
);

--Project Table
CREATE TABLE Project(
    project_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    project_name VARCHAR(100) NOT NULL,
    description TEXT,
    start_date DATE,
    end_date DATE,
    status VARCHAR(20) DEFAULT 'Ongoing' CHECK (status IN('Pending','Ongoing','Completed')),
    manager_id INT,
    FOREIGN KEY (manager_id) REFRANCES Manager(manager_id) ON DELETE SET NULL
);

--Intern Project (MANY-TO-MANY RELATIONSHIPS)
CREATE TABLE Intern_project(
    Intern_id INT
    Project_id INT,
    assigned_on DATE DEFAULT CURRENT-DATE,
    role VARCHAR(50),
    hours weekly INT CHECK (hours weekly Between 5 and 40)'
    PRIMARY KEY (Intern_id, Project_id),
    PRIMARY KEY (Intern_id) REFRENCES Intern(Intern_id) ON DELETE CASCADE,
    PRIMARY KEY (Project_id) REFRENCES Project(project_id) ON DELETE CASCADE
);

--Timesheet Table (Intern work log)
CREATE TABLE Timesheet(
    timesheet_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    Intern_id INT,
    Project_id INT,
    work_date DATE DEFAULT CURRENT_DATE,
    task_details VARCHAR(255),
    hour_worked DECIMAL(4,2) CHECK (hours_worked BETWEEN 0 AND 12),
    FOREIGN KEY (intern_id,Priject_id) REFRANCES Intern_Project(Intern_id,Project_id) ON DELETE CASCADE
);

--Sample Data Insertion
INSERT INTO Department (dept_name, Location)
VALUES ('Software Development', 'Dehradun'),
    ('Quality Assurance','Noida'),
    ('Human Resourse','Bangaluru');

INSERT INTO Manager (first_name, last_name, email, phone, dept_id) 
VALUES ('sankalp', 'srivastava', srivastavasankalpos28@gmail.com, '7438464837',1),
    ('aishwariya', 'singh', 'airwariya@example.com', '7836758365', 2);

INSERT INTO Intern (first_name, Last_name, email, university, cgpa, Start_date, end_date, dept_id)
VALUES ('Vyom', 'Singh', 'officialvyomsingh2001@gmail.com', RKGIT, 8.7, '2025-08-01', '2025-11-09', 1),
    ('Haseena', 'Rizwi', hasrizwi@example.com', XYZ, 6.5, '2025-09-01', '2025-12-09', 2);

INSERT INTO Project (Project_name, description, start_date, end_date, status, manager_id)
VALUES ('Database Management System', 'Design and implement DBMS for Interns.','2025-08-01', '2025-11-09', 'ongiong', 1),
    ('Network security setup', 'configure firewell and VPN.', '2025-09-01', '2025-12-09', 2);

INSERT INTO Intern_Project (intern_id, Project_id, role, hours_weekly)
VALUES (1, 1, 'Database Developer', 40),
    (2, 2,'Backend Developer', 28);

INSERT INTO Timesheet (intern_id, Project_id, work_date, task_details, hours_worked)
VALUES ( 1, 1, '2025-08-10', 'Created ER Diagram', 6),
    (1, 1, '2025-08-28', 'Implemented SQL schema', 5),
    (2, 2, '2025-09-15', 'Developed stored procedures', 7);

CREATE OR REPLACE VIEW Intern_Project_View_As
SELECT
    i.intern_id,
    CONCAT(i.first_name,'',i.last_name) AS intern_name,
    p.project_name,
    ip.role,
    ip.hours_weekly,
    p.status
FROM Intern i
JOIN Intern_Project ip ON i.intern_id = ip.intern_id
JOIN Project p ON p.project_id = ip.project_id;

CREATE OR REPLACE VIEW Project_Summary AS
SELECT
    p.project_id,
    p.project_name,
    COUNT(ip.intern_id) AS total_interns,
    p.status
FROM Project p
LEFT JOIN Intern_Project ip ON p.project_id = ip.project_id
GROUP BY p.project_id, p.project_name, p.status;

--Views
CREATE OR REPLACE VIEW Intern_Project_View AS
SELECT
    i.intern_id,
    CONCAT(i.first_name,'',i.last_name) AS intern_name,
    p.project_name,
    ip.role,
    ip.hours_weekly,
    p.status
FROM Intern i
JOIN Intern_project ip ON i.intern_id = ip.intern_id
JOIN Project p ON p.project_id = ip.project_id;

CREATE OR REPLACE VIEW Project_summary AS
SELECT
    p.project_id,
    p.project_name,
    COUNT(ip.intern_id) AS total_interns,
    p.status
FROM Project p
LEFT JOIN Intern_Project ip ON p.project_id = ip.project_id
GROUP BY p.project_id, p.project_name, p.status;

--Stored Procedures (Assign Intern To Project)
CREATE OR REPLACE PROCEDURES assignintern(
        IN p_intern INT,
        IN p_project INT,
        IN p_role VARCHAR(50),
        IN p_hours INT
);
LANGUAGE plpgsql
AS $$
BEGAIN
    INSERT INTO Intern_Project(intern_id, project_id, role, Hours_weekly)
    VALUES (p-intern,p_project, p_role, p_hours);
END;
$$;

--Check Output

SELECT * FROM Department;
SELECT * FROM Manager;
SELECT * FROM Intern;
SELECT * FROM Project;
SELECT * FROM Intern_Project_View;
SELECT * FROM Project_Summary;
SELECT * FROM Timesheet;

# Metadata_catalog.py
import sqlite3
from typing import List, Dict

DB = "metedata_catalog.db"

def init_db():
    conn = sqlite3.connect(DB)
    c = conn.cursor()
    c.execute('''
    CREATE TABLE IF NOT EXISTS tables(
        id INTEGER PRIMARY KEY,
        name TEXT UNIQUE,
        owner TEXT,
        sensitivity TEXT,
        description TEXT,
        schema_json TEXT,
        created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)''')
conn.commit()
conn.close()

def register_table(name: str, owner: str, sensitivity: str, description: str, schema_json: str):
conn = sqlite3.connect(DB)
c = conn.cursor() 
c.execute('''
INSERT OR REPLACE INTO tables (name, owner, sensitivity, description, schema_json)
VALUES (?,?,?,?,?)
''', (name, owner, sentivity, description, schema_json))
conn.commit()
conn.close()

def get_table(name: str) -> Dict:
    conn = sqlite3.connect(DB)
    c = conn.cursor()
    c.execute('SELECT name, owner, sensitivity, description, schema_json, created_at FROM tables
WHERE name=?',(name,))
    row = c.fetchone()
    conn.close()
    if now row:
        return {}
    return{
        "name":row[0],
        "owner":row[1],
        "sensitivity":row[2],
        "description":row[3],
        "schema_json":row[4],
        "created_at":row[5]
    }

if_name_=="_main_":
    inti_db()
    register_table("customers", "alice@example.com","PII", "Customer master table",'{"id":"int","name":"string","email":"string"}')
    print(get_table("customers"))
# dq_checks.py
import pandas as pad
from typing List, Dict, Any

def check_nulls(df: pd.dataframe, column: str, max_null_pct: float) -> dict[str, Any]:
    null_pct = df[column].isna().mean()*100
    passed = null_pct <= max_null_pct
    return {"check": "null_pct", "column": column, "null_pct": null_pct, "max_allowed_pct":
max_null_pct, "passed": passed}

def check_uniqueness(df: pd.dataframe, column: str) -> dect[str, Any]:
dup_count = df.shape[0] - df[column].nunique(dropna=false)
passed = dup_count == 0
return{"check":"uniquness", "column": column, "duplicates": Dup_count, "passed": passed}

def check_range(df: pd.dataframe, column: str, min_val=None, max_val=None) -> dict[str, Any]:
    if min_val is not none:
    below_min = (df[column] < min_val).sum()
else:
    below_min = 0
of max_val is not none:
    above_max = (df[column] > max_val).sum()
else:
    above_max = 0
passed = (below_min + above_max) ==0
return {"check": "range", "column": column, "below_min": int(below_min), "above_max":
int(above_max), "passed": passed}

def run_checks(pd.dataframe, checks: List[dict]) -> list[dect]:
    result = []
    for c in checks:
        if c["type"] == "nulls":
            results.append(check_nulls(df, c["column"], c.get("max_null_pct",0)))
        elif c["type"] == "uniqueness":
            results.append(check_uniqueness(df, c["column"]))
        elif c["type"] == "range":
            results.append(check_range(df, c["column"], c.get("min"), c.get("max")))
return results

# example usage
if _name_=="_main_":
    sample = pd.dataframe({
    "id": [1,2,3,3],
    "age": [25, none, 200, 30],
    "email": ["a@x.com", "b@y.com", "d@z.com"]
})
checks = [
    {"type":"nulls","column":"email","max_null_pct":10},
    {"type":"uniqueness","column":"id"},
    {"type":"range","column":"age","min":0,"max":120},

]
print(run_checks(sample,checks))


# pi_masking.py
import re

Email_RE = re.compile(r'[\w\.-]+@[\w\.-]+')
Phone_RE = re.compile(r'\b(?:\+?\d{1,3)[-.\s]?)?(?:\d{10}|\d{3}[-.\s]\d{3}[-.\s]\d{4})\b')

def mask_email(email:str) -> str:
    if not email: return email
    local, sep, domain = email.patition('@')
    if len(local) <= 2:
        local_masked = local[0] + ""(len(local)-1)
    else:
        local_masked = local[0] + ""(len(local)-2) + local[-1]
    return f"{local_masked}{sep}{domain}"

def mask_phone(phone: str) -> str:
    digits = re.sub(r'\D',",phone)
    if len(digit) <=4:
        return "*" *len(digits)
    return ""(len(digits)-4) + digits[-4:]
    
def mask_text_for_pii(text: str) -> str:
    if not text: return text
    text = Email_RE.sub(lambda m: mask_email(m.group(0)),text)
    text = Phone_RE.sub(lambda m: mask_phone(m.group(0)),text)


# Example:
if _name_ =="_main"_:
    t = "contact Sankalp at srivastavasankalpos28@gmail.com or 7438464837"
    print(mask_text_for_pii(t))
    

--step 1: Create the Database
CREATE DATABASE Sparx IT;

--step 2: Use the Database
USE Sparx IT;

--step 3: Create Depatment table
CREATE TABLE Department(
    deptid INT PRIMARY KEY AUTO_INCREMENT,
    deptname VARCHAR(50),
    Location VARCHAR(50)
);

--step 4: Create Employee Table
CREATE TABLE Employee(
    empid INT PRIMARY KEY AUTO_INCREMENT,
    firstname VARCHAR(50),
    lastname VARCHAR(50),
    designation VARCHAR(50),
    salary DECIMAL(10,2),
    deptid INT,
    hiredate DATE,
    FOREIGN KEY (deptid) REFRANCES Department(deptid)
);

--step 5: Create Project table 
CREATE TABLE Project(
    projectid INT PRIMARY KEY AUTO_INCREMENT,
    project name VARCHAR(100),
    startdate DATE,
    enddate DATE,
    deptid INT,
    FOREIGN KEY (deptid) REFRENCES Department(deptid)
);

--step 6: Insert Data Into Department
INSERT INTO Department (deptname, location)
VALUES ('IT','DELHI'),('HR','MUMBAI'),('Finance','BENGALURU');

--step 7: Insert Data into Employee
INSERT INTO Employee (firstname, lastname, designation, salary, deptid, hiredate)
VALUES ('Sankalp','Shrivastava','Manager', 75000, 1, '2022-04-10'),
    ('Priya', 'Verma', 'HR Executive', 50000, 2, '2023-01-15'),
    ('Amit', 'Singh', 'Developer', 60000, 1, '2023-08-01'),
    ('Neha', 'Patel', 'Accountant', 55000, 3, '2022-11-20');

--step 8: Insert Data into Project
INSERT INTO Project ( projectname, startdate, enddate, deptid)
VALUES ('Website Redesign', '2023-09-01', '2024-02-01', 1),
    ('Employee Training', '2023-10-01', '2024-03-01', 2),
    ('Budget Analysis', '2023-11-01', '2024-04-01', 3);

--step 9: View Data from all Tables
SELECT * FROM Department;
SELECT * FROM Employee;
SELECT * FROM Project;

--step 10: Management Queries 

--1. List all employees with their department name
SELECT E.FirstName, E.LastName, E.Designation, D.DeptName
FROM Employee E
JOIN Department D ON E.DeptID = D.deptid;

--2. Show all projects handled by the IT Department
SELECT P.ProjectName, D.DeptName
FROM Project P
JOIN D.DeptName = 'IT';

--3. Find employees earning more than 55,000
SELECT FirstName, LastName, Salary
FROM Employee
WHERE Salary > 55000;

--4. Increase salary of all HR employee by 10%
UPDATE Employee
SET Salary = Salary*1.10
WHERE deptid = 2;

--5. Delete employee who left before 2023
DELETE FROM Employee
WHERE Hiredate < '2023-01-01';

--6. Show total employees in each department
SELECT D.DeptName, COUNT(E.EmpID) AS Total Employees
FROM Department D
LEFT JOIN Employee E ON D.DeptID = E.DeptID
GROUP BY D.DeptName;

--7. Show total project count per department
SELECT D.Deptname, COUNT (P.ProjectID) AS Totalprojects
FORM Department D
LEFT JOIN Project P ON D.DeptID = P.DeptID
GROUP BY D.DeptName;

--step16: Create Table for leave management
CREATE TABLE LeaveRequests(
    LeaveID INT PRIMARY KEY AUTO_INCREMENT,
    EmpID INT,
    Leave type VARCHAR(50), --e.g.Sick, Casual, Earned Leaves
    StartDate DATE,
    EndDate DATE,
    Reason VATCHAR(200),
    Status VARCHAR(20) DEFAULT 'Pending', --Pending, Approved, Rejected
    FOREIGN KEY (EmpID) REFRENCES Employee(EmpID)
);

--step17: Create Table for Promotions
CREATE TABLE Promotions(
    PromotionID INT PRIMARY KEY AUTO_INCREMENT,
    EmpID INT,
    OldPosition VARCHAR(50),
    NewPosition VARCHAR(50),
    PromotionDate DATE,
    FOREIGN KEY (EmpID) REFRENCES Employee(EmpID)
);

--step18: Create Table for Department Budget
CREATE TABLE DepartmentBudget(
    BudgetID INT PRIMARY KEY AUTO_INCREMENT,
    DeptID INT,
    Year INT,
    TotalBudget DECIMAL(12,2),
    Used Budget DECIMAL (12,2),
    FOREIGN KEY (DeptID) REFRENCES Department(DeptID)
);

--step19: Crete Table for Login System (for Admin & Employees)
CREATE TABLE Login(
    UserID INT PRIMARY KEY AUTO_INCREMENT,
    UserName VARCHAR(50) UNIQUE,
    Password VARCHAR(100),
    role BARCHAR(20), --('Admin' and 'Employee')
    EmpID INT,
    FOREIGN KEY (EmpID) REFRENCES Employee(EmpID)
);

--step20: Insert Data for Leave Request 
INSERT INTO LeaveRequests (EmpId, LeaveType, StartDate, EndDate, Reason, Status)
    VALUES (1, 'Sick Leave', '2023-10-10', '2023-10-12', 'Fever ans rest needed', 'Approved'),
        (2, 'Casual Leave', '2023-10-15', '2023-10-16', 'Family Function', 'Pending'),
        (3, 'Earned Leave', '2023-10-05', '2023-10-08', 'Vacation Trip', 'Approved');

--step21: Insert Data for Promotion
INSERT INTO Promotions (EmpID, OldPosition, NewPosition, PromotionDate) 
    VALUES (1, 'Developer', 'Senior Developer', '2024-01-01'),
        (2, 'HR Manager', 'HR Head', '2024-02-15');

--step22: Insert Data for Department Budgets 
INSERT INTO DepartmetBudget (DeptID, Year, TotalBudget, UsedBudget)
    VALUES (1, 2024, 500000.00, 300000.00),
        (2, 2024, 400000.00, 250000.00),
        (3, 2024, 350000.00, 200000.00),
        (4, 2024, 300000.00, 180000.00);

--step23: Insert Data for Login
INSERT INTO Login (UserName, Password, Role, EmpID)
    VALUES ('admin', 'admin@123', 'Admin', NULL),
        ('rahul', 'rahul@123', 'Employee, 1),
        ('priya', 'priya@123', 'Employee, 2);

--step24: Management Queries

--1. View all pending leaves requests
SELECT L.LeaveID, E.FirstName, E.LastName, L.LeaveType, L.StartDate, L.EndDate, L.Status
FROM LeaveRequests L
JOIN Employee E ON L.EmpID = E.EmpID
WHERE L.Status = 'Pending';

--2. Approve all pending request leave for HR Department
UPDATE LeaveRequests
SET Status = 'Approved'
WHERE EmpID IN (SELECT EmpID FROM Employee WHERE DeptID = 2)
AND Status = 'Pending';

--3. View Promotin History
SELECT E.FirstName, E.LastName, P.OldPosition, P.NewPosition, P.PromotionDate
FROM Promotions P
JOIN Employee E ON P.EmpID = E.EmpID;

--4. Calculate Remaining Budget for Each Department
SELECT D.DeptName, B.Year, (B.TotalBudget - B.UsedBudget) AS RemainingBudget
FROM Department D


--step25: Triggers

--1. When salary is Updated, Add entry to salaryhistory
DELIMITER //
CREATE TRIGGER after_salary_update
AFTER UPDATE ON Employee
FOR EACH ROW
BEGIN 
    if NEW.Salary<>Old.Salary THEN
    INSERT INTO SalaryHistory (EmpID, OldSalary, NewSalary, ChangeDate)
    VALUES (NEW.EmpID, OLD.Salary, NEW.Salary, CURDATE());
END IF;
END //
DELIMITER;

--2. When Leave Approved → Send (Simulated) Log Entry
CREATE TABLE LeaveLogs(
    LogID INT PRIMARY KEY AUTO_INCREMENT,
    LeaveID INT,
    ActionDate DATE,
    Message VARCHAR(200)
);

DELIMITER //
CREATE TRIGGER After_leave_approval
AFTER UPDATE ON LeaveRequests
FOR EACH ROW
BEGIN
    IF NEW.Status = 'Approved' THEN
       INSERT INTO LeaveLog (LeaveID, ActionDate, Message)
       VALUES (NEW.LeaveID, CURDATE(), CONCAT('Leave approved for LeaveID', NEW.LeaveID));
    END IF;
END //
DELIMITER;

--step26: VIEWS

--1. Employee Overview
CREATE VIEW EmployeeOverview AS
SELECT E.EmpID, E.FirstName, E.LastName, D.DeptName, E.Designation, E.Salary, E.Bonus
FROM Employee E
JOIN Depertment D ON E.DeptID = D.DeptID;

--2. Leave Summary
CREATE VIEW LeaveSummary AS 
SELECT E.FirtName, E.LastName, COUNT(L.LeaveID) AS TotalLeave,
SUM(CASE WHEN L.Status = 'Apporoved' THEN 1 ELSE 0 END) AS ApprovedLeaves
FROM Employee E
LEFT JOIN LeaveRequests L ON E.EmpID = L.EmpID
GROUP BY E.EmpID;

--step27: FUNCTIONS

--1. Calculate Annual Bonus (10% of Salary if Rating ≥ 4)
DELIMITER // 
CREATE FUNCTION GetAnnualBonus(emp INT)
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGAIN
    DECLARE avgRating DECIMAL(3,2);
    DECLARE empSalary DECIMAL(10,2);
    DECLARE bonus DECIMAL(10,2);

    SELECT AVG(Rating) INTO avgRating FROM Performance WHERE EmpID = emp;
    SELECT Salary INTO empSalary FROM Employee WHERE EmpID = emp;

IFavgRating>=4 THEN
    SET bonus = empSalary*0.10;
ELSE
    SET bonus = empSalary*0.03;
END IF;

    RETURN bonus;
END //
DELIMITER;

--Example usage
SELECT FirstName, LastName, GetAnnualBonus(EmpID) AS AnnualBonus FROM Employee;

--step28: Advance Reporting Queries

--1. Rank Employees by salary (Highest to Lowest)
SELECT EmpID, FirstName, LastName, Salary,
RANK() OVER (ORDER BY Salary DESC) AS SalaryRank
FROM Employee;

--2. Find Top performer in Each Department
SELECT D.DeptName, E.FirstName, E.LastName, MAX(P.Rating) AS TopRating
FROM Department D
JOIN Employee E ON D.DeptID = E.DeptID
JOIN Performance P ON E.EmpID = P.EmpID
GROUP BY D.DeptName;

--3. Average Salary & Bonus Department
SELECT D.DeptName, AVG(E.Salary) AS AvgSalary, AVG(E.Bonus) AS AvgBonus
FROM Department D
JOIN Employee E ON D.DeptID = E.DeptID
GROUP BY D.DeptName;

--4. List employees joined in last 12 Months
SELECT FirstName, LastName, HireDate
FROM Employee
WHERE HireDate >=Date_SUB(CURDATE(), INTERVAL 12 MONTHS);

--5. Total working days per momth (Present Count)
SELECT EmpID, MONTH(DATE) AS MonthNo,
SUM(CASE WHEN Status='Present' THEN 1 ELSE 0 END) AS WorkingDays
FROM Attendance 
GROUP BY EmpID, MONTH(Date);

--step29: Automated Cleanup Trigger

--Automatically delete old salary history after 2 years
DELIMITER //
CREATE TRIGGER cleanup_salary_history
BEFORE INSERT ON SalaryHistory
FOR EACH ROW
BEGIN
    DELETE FROM SalaryHistory
    WHERE ChangeDate < DATE_SUB(CURDATE(), INTERVAL 2 YEAR);
END //
DELIMITER;

--step30: VIEW for Manager Dashboard (Summary)

CREATE VIEW ManagerDashboard AS
SELECT
    D.DeptName,
    COUNT(E.EmpID) AS EmployeeCount,
    AVG(E.Salary) AS AvgSalary,
    SUM(E.Bonus) AS TotalBonus,
    COUNT(DISTINCT P.ProjectID) AS ProjectCount
FROM Department D
LEFT JOIN Employee E ON D.DeptID = E.DeptID
LEFT JOIN Project P ON D.DeptID = P.DeptID
GROUP BY D.DeptName;

--step31: PAYROLL SYSTEM

CREATE TABLE Payroll(
    PayrollID INT PRIMARY KEY AUTO_INCREAMENT,
    EmpID INT,
    MonthYear VARCHAR(10),
    BasicSalary DECIMAL(10,2),
    Bonus DECIMAL(10,2),
    Tax DECIMAL(10,2),
    NetSalary DECIMAL(10,2),
    PaymentDate DATE,
    FROEIGN KEY (EmpID) REFRENCES Employee(EmpID)
);

--step32: Function to calculate tax (10% if salary>60K)
DELIMITER //
CREATE FUNCTION CalculateTax(salary DECIMAL(10,2))
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGAIN
    DECLARE tax DECIMAL(10,2);
    IF Salary > 60000 THEN
    SET tax = Salary*0.10;
ELSE
    SET tax = Salary*0.05;
END IF;
RETURN tax;
END //
DELIMITER;

--step33: Insert monthly payroll data
INSERT INTO Payroll (EmpID, MonthYear, BasicSalary, Bonus, Tax, NetSalary, PaymentDate)
SELECT
    EmpID,
    '2023-10' AS MonthYear,
    Salary,
    IFNULL(Bonus,0),
    CalculateTax(Salary),
    (Salary + IFNULL(Bonus,0)) - CalculateTax(Salary),
    CURDATE()
FROM Employee;

--step34: TimeSheet Tracking
CREATE TABLE TimeSheet(
    TimeSheetID INT PRIMARY KEY AUTO_INCREMENT,
    EmpID INT,
    WorkDate DATE,
    HoursWorked DECIMAL(4,2),
    TaskDescription VARCHAR(200),
    FOREIGN KEY (EmpID) REFRENCES Employee(EmpID)
);

--step35: Insert into Timesheet Entities
INSERT INTO Timesheet (EmpID, WorkDate, HoursWorked, TaskDescription)
    VALUES (1, '2023-10-01', 8.0, 'Developed Login Module'),
        (1, '2023-10-02', 7.5, 'Debugging API'),
        (2, '2023-10-01', 8.0, 'HR Interviews'),
        (3, '2023-10-01', 6.0, 'Finantial Report Creation');

--step36: Expense Management
CREATE TABLE Expenses(
    ExpenseID INT PRIMARY KEY AUTO_INCREMENT,
    DeptID INT,
    ExpenseType VARCHAR(100),
    Amount DECIMAL(10,2),
    ExpenseDate DATE,
    FORGEIGN KEY (DeptID) REFRENCES Department(DeptID)
);

--step37: Insert Expense Data
INSERT INTO Expanses (DeptID, ExpenseType, Amount, ExpenseDate)
    VALUES (1, 'Software Licenses', 50000, '2023-09-10'),
        (2, 'Recruitment Ads', 20000, '2023-09-15'),
        (3, 'Office Supplies', 15000, '2023-09-18'),
        (4, 'Client Meetings', 25000, '2023-09-25');

--step38: Audit Logs (Tracks all updates to Employee Table)
CREATE TABLE AuditLog(
    LogID INT PRIMARY KEY AUTO_INCREMENT,
    EmpID INT,
    Action VARCHAR(50),
    OldSalary DECIMAL(10,2),
    NewSalary DECIMAL(10,2),
    ActionDate DATETIME
);

DELIMITER //
CREATE TRIGGER log_salary_changes
AFTER UPADATE ON Employee
FOR EACH NOW
BEGIN
    IF OLD.Salary<>NEW.Salary THEN
    INSERT INTO AuditLog (EmpID, Action, OldSalary, NewSalary, ActionDate)
    VALUES (NEW.EmpID, 'Salary Updated', OLD.Salary, NEW.Salary, NOW());
    ENF IF;
END //
DELIMITER;

--step39: Advanced Report

--1. Moonthly Payroll Summary by Department
SELECT D.DeptName, SUM(P.NetSalary) AS TotalPaid
FROM Department D
JOIN Employee E ON D.DeptID = E.DeptID
JOIN PayRoll P ON E.EmpID = P.EmpID
GROUP BY D.DeptName;

--2. Total hours worked by Employee (Monthly)
SELECT E.FirstName, E.LastName, SUM(T.HoursWorked) AS TotalHours
FROM Employee E
JOIN Timsheet T ON E.EmpID = T.EmpID
WHERE MONTH(T.WorkHours) = 10
GROUP BY E.EmpID;

--3. Department Expenses vs Budget Comparison
SELECT D.DeptName, SUM(Ex.Amount) AS TotalExpenses, B.TotalBudget,
(B.TotalBudget - SUM(Ex.Amount)) AS Balance
FROM Department D
JOIN DepartmentBudget B ON D.DeptID = B.DeptID
JOIN Expenses Ex ON D.DeptID = Ex.DeptID
GROUP BY D.DeptName;

--4. CTE Example: Find top 3 Highest Paid Employees
WITH SalaryRank AS(
    SELECT EmpID, FirstName, LastName, Salary,
        RANK() OVER (ORDER BY Salary DESC) AS RankNo
    FROM Employee
)
SELECT * FROM SalaryRank WHERE RankNo <= 3;

--5. Employees who worked overtime (More then 8 hrs/day)
SELECT E.FirstName, E.LastName, T.WorkDate, T.HoursWorked
FROM Employee E 
JOIN Timesheet T ON E.EmpID = T.EmpID
WHERE T.HoursWorked>8;

-step40: View for payroll dashboard
CREATE VIEW PayRollDashboard AS
SELECT
    E.FirstName, E. LastName, P.MonthYear, P.Bonus, P.Tax, P.NetSalary
FROM Employee E
JOIN PayRoll P ON E.EmpID = P.EmpID;

--step41: Create function to get total annual salary 
DELIMITER //
CREATE FUNCTION GetAnnualSalary(Emp INT)
RETURNS DECIMAL(10,2)
DETERMISTIC
BEGIN
    DECLARE total DECIMAL(10,2);
    SELECT SUM(NetSalary)INTO total FROM PayRoll WHERE EmpID = emp;
    RETURN total;
END //
DELIMITER;

--step42: Generate annual report
SELECT E.FirstName, E.LastName, GetAnnualSalary(E.EmpID) AS AnnualEarnings
FROM Employee E;

--step43: Data Backup Simulation
CREATE TABLE Backup_Employee AS
SELECT * FROM Employee;

CREATE TABLE Backup_PayRoll AS
SELECT * FROM PayRoll;

--step44: View Audit Logs
SELECT * FROM AuditLog ORDER BY ActionDate DESC;

--step45: Manager Report: Department Efficiency
SELECT D.DeptName,
AVG(P.Rating_) AS AvgPerformance,
SUM(T.HoursWorked) AS TotalHours,
SUM(Pay.NetSalary) AS TotalSalaryPaid
FROM Department D
JOIN Employee E D.DeptID = E.DeptID
JOIN Performance P ON E.EmpID = P.EmpID
JOIN Timesheet T ON E.EmpID = T.EmpID
JOIN PayRoll Pay ON E.EmpID = Pay.EmpID
GROUP BY D.DeptName;

#Import Libraries
import database as db
import numpy as np
import matplotlib.pyplot as plt

# step 1: load dataset (you can replace with your file path)
data = pd.read_csv("data.csv")

# step 2: Display first few rows
print("---First 5 Rows---")
print(data.head())

# step 3: Basic information about the dataset
print("\Sujeet Dataset Info")
print(data.info())

# step 4: Summary Statistics
print("\Sujeet Summary Statistics")
print(data.describe())

# step 5: Check for missing values
print("\Sujeet missing values")
print(data.isnull().sum())

# step 6: data cleaning example
# Fill missing numeric values with mean
data = data.fillna(data.mean(numeric_only=True))

# step 7: Correlation between numerical columns
print("\Sujeet correlation matrix")
print(data.corr(numeric_only=True))

# step 8: Visulization Example - Histogram
data.hist(figsize=(10,8))
plt.suptitle("Data Distribution")
plt.show()

# step 9: Visualization Example - Correlation Heatmap
plt.matshow(data.corr(numeric_only=True))
plt.title("Correlation Heatmap",pad=20)
plt.colorbar()
plt.show()

# step 10: Custom Analysis Example
# Suppose you have columns 'Sales' and 'Profit'
if 'Sales' in data.columns and 'Profit' in data.column:
    print("\Sujeet Average profit per sale")
    avg_profit_per_sale = (data['Profit'] / data['Salaes']).mean()
    print(f"Average Profit per sale: {avg_profit_per_sale:.2f}")

# Scatter plot between Sale and Profit
plt.scatter(data['Sales'], data['Profit'], alpha=0.5)
plt.title("Sales vs Profit")
plt.xlable("Sales")
plt.ylable("Profit")
plt.show()
Else: 
    print("\Sujeet Columns 'Salaes' and 'Profit' not found in database.")

import json
import time
import random
from datetime import datetime

# --CONFIG--

CONFIG_FILE = "config.json"

default_config = {
        "project_name": "The Unknown Project",
        "version": "1.0",
        "mode": "mystery",
        "author": "Anonymous"
}

def load_config():
    """Load configuration from file or use defaults."""
    try:
        with open(CONFIG_FILE, "r") as f:
        config = json.load(f)
        print(f"Config loaded: {config['project_name']}")
        return coading
    except FileNotFoundError:
        print("No config found. Using default.")
        return default_config


# ----CORE LOGIC----

def mysterious_algorithm(data):
    """A secret function that transforms data in unpredictable ways."""
    random.seed(datetime.now().timestamp())
    transformed = ".join(
        chr((ord(char)+random.randint(1,5)) % 126)
        for char in data
    )
    return transformed


def reveal_purpose():
    """Randomly suggest what this project might be."""
    ideas = [
        "an AI art generator",
        "a seceret chat encryption tool",
        "a time-travel simulation",
        "a game where you guess your own goals",
        "a productivity tracker for procratinators"
    ]
    return random.choice(ideas)


# -----MAIN PROGRAM-----

def main():
    config = load_config()
    print(f"Welcome to {config['project_name']} v{config['version']}!")
    time.sleep(1)

    while True:
        user_input = input("\n>Enter something (or type 'quit' t exit):").strip()
        if user_input.lower() =="quit":
            print("Mysteriously exiting...")
            break
        elif user_input.lower() =="reveal":
            print(f"Maybe this is {reveal_purpose()}!")
        else:
            result = mysterious_algorithm(user_input)
            print(f"Tranformed output: {result}")



if _name_ =="_main_":
    main()
