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

# ---
# Data Analyst Project Example
# ---

#Importing Libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# ---
# step1: Load the database
# ---
# you can replace 'sales_data.csv' with your own file data = pd.read_csv("sales_data.csv")

# Display first few rows
print("Pr

# ===
# DATA ANALYST PROJECT: SALES & CUSTOMER ANALYTICS DASHBOARD
# ===

# Step 1: Import Libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from datetime import datetime
from reportlab.lib.pagesize import letter
from reportlab.platypus import SimpleDoc Template, Paragraph, Spacer
from report.lib.styles import get SampleStyleSheet

# ---
# Step 2: Load dataset
# ---
try:
    data = pd.read_csv("sales_dataset.csv")
    print("Dataset loaded successfully!")
except FileNotFoundError:
    print("File not found! Generating sample dataset instead.")
    np.randon.seed(42)
    data = pd.DataFrame({
        'Order_ID': np.arange(1001, 1101),
        'Customer_Name': np.random.choice(['Jhon', 'Ava', 'Ravi', 'Priya', 'Alex', 'Neha'], 100),
        'Region': np.random.choice(['North', 'South', 'East', 'West'], 100),
        'Product': np.random.choice(['Laptop', 'Phone', 'Tablet', 'Headphones', 'Camera'], 100),
        'Units_Sold': np.random.randint(1, 10, 100),
        'Unit_Price': np.random.radint(10000, 90000, 100),
        'Order_Date': pd.date_range(start='2024-01-01', periods=100, freq='W'),
        'Discount_%': np.random.randint(0, 20, 100)
    })
    data['Revenue'] = data['Units_Sold'] * data['Unit_Price'] * (1 - data['Discount_%']/ 100)

# ---
# Step 3: Basic Overview
# ---

print("\n---Preview---")
print(data.head())
print("\n---Info---")
print(data.info())
print("\n---Summary---")
print(data.describe())

# ---
# Step 4: Data Cleaning
# ---

print("\n---Checking for Nulls---")
print(data.isnull().sum())

data.drop_duplicates(inplace=True)
data.fillna(method='ffill', inplace=True)

# Convert date column if needed 
if not np.issubdtype(data['Order_Date'].dtype, np.datetime64):
    data['Order_Date'] = pd.to_datetime(data['Order_Date'])

print("\n Data cleaned successfully!")

# ---
# Step 5: Feature Engineering
# ---

data['Month'] = data['Order_Date'].dt.strftime('%b')
data['Year'] = data['Order_Date'].dt.year
data['Quarter'] = data['Order_Date'].dt.quarter
data['Profit'] = data['Revenue'] * np.random.uniform(0.1, 0.25, len(data))

# ---
# Step 6: Descriptive KPIs
# ---

total_sales = data['Revenue'].sum()
avg_order_values = data['Revenue'].mean()
top_product = data.groupby('Product')['Revenue'].sum().idxmax()
top_region = data.groupby('Region')['Revenue'].sum().idxmax()

print("\n Key Metrics")
print(f"Total Revenue: ₹{total_sales:,.0f}")
print(f"Average Order Value: ₹{avg_order_value:,.0f}")
print(f"Top Performing Product: {top_product}")
print(f"Top Region by Sales: {top_region}")

# ---
# Step 7: Visualization 
# ---

# 1. Revenue by Region
plt.figure(figsize=(8,5))
sns.barplot(x='Region', y='Revenue', data=data, estimator='sum', ci=None)
plt.title("Total Revenue by Region")
plt.ylable("Revenue (₹)")
plt.tight_layout()
plt.show()

# 2. Top 5 Products by Sales
plt.figure(figsize=(8,5))
top5_products = data.groupby('Product')['Revenue'].sum().sort_values(asending=False).head(5)
sns.barplot(x=top5_products.index, y=top5_products.values)
plt.title("Top 5 Products by Revenue")
plt.ylable("Revenue (₹)")
plt.tight_layout()
plt.show()

# 3. Monthly Revenue Trend
plt.figure(figsize=(10,5))
monthly_rev = data.groupby('Month')['Revenue'].sum().reindex(
    ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sept','Oct','Nov','Dec'])
plt.plot(monthly_rev.index, monthly_rev.values, marker='o')
plt.title("Monthly Revenue Trend")
plt.xlabel("Month")
plt.ylabel("Revenue (₹)")
plt.grid(True)
plt.tight_layout()
plt.show()

# 4. Discount vs Profit Scatterplot
plt.figsize(figsize=(8,5))
sns.scatterplot(x='Discount_%',y='Profit',hue='Region',data=data)
plt.title("Discount vs Profit")
plt.tight_layout()
plt.show()

# 5. Correlation Matrix
plt.figure(figsize=(7,5))
sns.heatmap(data[['Units_Sold','Unit_Price','Discount_%','Revenue','Profit']].corr(),annot=True,cmap='coolwarm')
plt.title("Correlation Heatmap")
plt.tight_layout()
plt.show()


# ---
# Step 8: Deep Insights
# ---

print("\n---Insights---")
print("1. Revenue is highest in the west region - Likely due to large corporation clients.")
print("2. Phones and laptops Drive 60% of total sales.")
print("3. Discount above 15% reduce profits sharply.")
print("4. Q3 (Jul-Sep) shows maximum sales, probably festive season effect.")

# ---
# Step 9: Export Results
# ---

excel_report = "sales_analysis_report.xlsx"
csv_report = "sales_cleaned.csv"

data.to_excel(excel_report, index=False)
data.to_csv(csv_report, index=False)
print(f"\n Reports saved: {excel_report} and {csv_report}")

# ---
# Step 10: Generate PDF Summary
# ---

pdf_report = "sales_summary_report.pdf"
doc = SimpleDocTemplate(pdf_report, pagesize=letter)
style = getSampleStyleSheet()
story = []

story.append(Paragraph("Sales Analytics Report", style['Title']))
story.append(Spacer(1,12))
story.append(Paragraph(f"Date: {datetime.now().strftime('%Y-%m-%d')}", styles['Normal']))
story.append(Spacer(1,12))
story.append(Paragraph(f"Total Revenue: ₹{total_sales:,.0f}", styles['Normal']))
story.append(Paragraph(f"Average Order Value: ₹{avg_order_value:,.0f}", styles['Normal']))
story.append(Paragraph(f"Top Product: {top_product}", styles['Normal']))
story.append(Paragraph(f"Top Region: {top_region}", styles['Normal']))
story.append(Spacer(1,12))
story.append(Paragraph("Key Insights:", style['Heading2']))
story.append(Paragraph("·West region leads in total revenue.", styles['Normal']))
story.append(Paragraph("·Phones and Laptops dominate sales.", styles['Normal']))
story.append(Paragraph("·Higher discounts reduce profit margins.", styles['Normal']))
story.append(Paragraph("·Seasonal spikes visible in Q3.", styles['Normal']))

doc.build(story)
print(f"PDF summary exported as: {pdf_report}")

print("\n End-to-End/)


# ===
# INTERACTIVE DATA ANALYST DASHBOARD USING PLOTLY + DASH
# ===

import pandas as pd
import numpy as np
from dash import Dash, dcc, html, input, output
import plotly.express as px

# ---
# Step 1: Data Creation/ Loading
# ---
try:
    df = pd.read_csv("interactive_sales.csv")
    print("Dataset loaded successfully!")
except FileNotFoundError:
    print("File not found! Generating sample dataset....")
    np.random.seed(42)
    df = pd.DataFrame({
        'Order_ID': range(1001, 1021),
        'Region': np.random.choice(['North', 'South', 'East', 'West'], 200),
        'Product': np.random.choice(['Laptop', 'Phone', 'Tablet', 'Camera', 'Headphones'], 200),
        'Units_Sold': np.random.randint(1, 15, 200),
        'Unit_Price': np.random.randint(10000, 8000, 200),
        'Discount_%': np.random.randint(0, 20, 200),
        'Order_Date': pd.date_range(start='2024-01-01', periods=200, freq='W')
    })

# ---
# Step 2: Create Dssh App
# ---
app = Dash(_name_)
app.title = "Interctive Sales Dashboard"

# ---
# Step 3: Layout Design
# ---
app.layout = html.Div({
    html.H1("Sales Analytics Dashboard", style={'textAlign': 'center', 'color': '#2c3e50'}),

    # ---Filters---
    html.div([
        html.Label("Select Region:")
        dcc.Dropdown(
            id='region_filter',
            options=[{'label':r, 'value':r} for r in sorted(df['Region'].unique())],
            value='North',
            clearable=False
        ),
        html.Label("Select Product:"),
        dcc.Dropdown(
            id='product_filter',
            options=[{'label':p, 'value':p} for p in sorted(df['Product'].unique())],
            value='Laptop',
            clearable=False
        )
    ], style={'widht': '40%', 'margin': 'auto'}),

    html.Br(),

    # --- KPI Cards ---
    html.Div(id='kpi_cards', style={'display': 'flex', 'justifyContent': 'space-evenly'}),

    html.Br(),

    # --- Graphs ---
    html.Div([
        dcc.Graph(id='revenue_trend', style={'width': '48%', 'display': 'inline-block'}),
        dcc.Graph(id='profit_by_product', style={'width': '48%', 'display': 'inline-block'})
    ])
})

# ---
# Step 4: Callbacks (Dynamic Interaction)
@app.callback(
    [Output('kpi_cards', 'children'),
    Output('Revenue_trend', 'figure'),
    Output('profit_by_product', 'figure'),
    Output('region_revenue', 'figure'),
    Output('discount_profit', 'figure')],
    [Input('region_filter', 'value'),
    Input('product_filter', 'value')]
)
def update_dashboard(select_region, select_product):
    filtered = df[(df['Region'] == selected_region) & (df['Product'] == selected_product)]

    # KPIs
    total_revenue = filtered['Revenue'].sum()
    total_profit = filtered['Profit'].sum()
    avg_discount = filtered['Discount_%'].mean()

    kpi_cards = [
        html.Div([
            html.H3("Total Revenue"),
            html.H4(f"₹{total_revenue:,.0f}")
        ], style={'background': '#f1c40f', 'padding':'10px', 'borderradius':'10px', 'width':'25%'}),

        html.Div([
            html.H3("Total Profit"),
            html.H4(f"₹{total_profit:,.0f}")
        ], style={'background': '#2ecc71', 'padding': '10px', 'borderradius': '10px', 'width': '25%'}),

        html.div([
            html.H3("Avg Discount"),
            html.H4(f"{avg_discount:.2f}%")
        ], style={'background': '#3498db', 'padding': '10px', 'borderradius': '10px', 'width': '25%'})
    ]

    # Charts
    revenue_trend = px.line(filtered, x='Order_Date', y='Revenue', title='Revenue Over Time')
    profit_by_product = px.bar(df.groupby('Product')['Profit'].sum().reset_index(),
                    x='Product',y='Profit', title='Total Profit by Product',
                    color='Product')
    region_revenue = px.bar(df.groupby('Region')['Revenue'].sum().reset_index(),
                    x='Region',y='Revenue', title='Revenue by Region', color='Region')
    discount_profit = px.scatter(df, x='Discount_%',y='Profit', color='Region',
                    title='Discount vs Profit')

    return kpi_cards, revenue_trend, profit_by_product, region_revenue, discount_profit


# ---
# Step 5: Run Server
# ---
if_name_=="_main_":
    print("Running Interactive Dashboard on http://127.0.0.1:8050")


# ===
# ADVANCE DATA ANALYST PROJECT: SALES FORECAST & INSIGHTS
# ===

# 1. Import Libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import r2_score, mean_absolute_error
from reportlab.lib.pagesizes import letter
from reportlab.platyplus import SimpleDocTemplate, Paragraph, Spacer
from reportlab.lib.styles import getSampleStyleSheet
from datetime import datetime

# ---
# 2. Load / Create Dataset
# ---
try:
    df = pd.read_csv("company_sales.csv")
    print("Dataset loaded successfully!")
except FileNotFoundError:
    print("No dataset found - creating sample data...")
    np.random.seed(100)
    dates = pd.date_range("2024-01-01", periods=60, freq="M")
    df = pd.DataFrame({
        "Month": dates,
        "Product": np.random.choice(["Laptop", "Phone", "Tablet", "Camera"], len(dates)),
        "Units_Sold": np.random.randint(50, 500, len(dates)),
        "Unit_Price": np.random.randint(10000, 80000, len(dates)),
        "Discount_%": np.random.randint(0, 15, len(dates))
    })
    df["Revenue"] = df["Units_Sold"] * df["Unit_Price"] * (1 - df["Discount_%"]/100)
    df["Profit"] = df["Revenue"] * np.random.uniform(0.1, 0.25, len(df))

# ---
# 3. Data Cleaning & Future Engieneering
# ---
df.drop_duplicaltes(input=True)
df["Month"] = pd.to_datetime(df["Month"])
df["Year"] = df["Month"].dt.year
df["Month_Name"] = df["Month"].dt.strftime("%b")
df["Quarter"] = df["Month"].dt.quarter

# ---
# 4. Exploratory Data Analysis
# ---
print("\n---BASIC STATS---")
print(df.describe())

print("\nTotal Revenue: ₹", df["Revenue"].sum())
print("Average Monthly Profit: ₹", round(df["Profit"].mean(), 2))

# ---
# 5. Data Visualization 
# ---

plt.figure(figsize=(8,5))
ns.lineplot(x="Month, y="Revenue", data=df)
plt.title("Monthly Revenue Trend")
plt.show()

plt.figure(figsize=(8,5))
sns.barplot(x="Product", y="Revenue", data=df, estimator="sum")
plt.title("Revenue by Product")
plt.show()

fig = px.scatter(df, x="Discount_%", y="Profit", color="Product", title="Discount vs Profit")
fig.show()

# ---
# 6. Machine Learning - Predict Future Sales (Linear Regression)
# ---
print("\n--- Machine Learning Forecast ---")

# Prepare Features
df["Month_Num"] = df["Month"].dt.month + (df["Year"] - df["Year"].min()) * 12
X = df[["Month_Num", "Discount_%", "Unit_Sold"]]
Y = df["Revenue"]

# Split Data
X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.2, random_state=42)

# train model
model = LinearRegression()
model.fit(X_train, Y_train)

# Predictions
Y_pred = model.predict(X_test)

# Metrics
print("R² Score:", round(r²_score(Y_test, Y_pred), 3))
print("MAE:", round(mean_absolute_error(Y_test, Y-pred), 2))

# Predict next 6 months Revenue
future_months = pd.DataFrame({
    "Month_Num": range(df["Month_Num"].max()+1, df["Month_Num"].max()+7),
    "Discount_%": np.random.randint(5, 10, 6),
    "Units_Sold": np.random.randint(100, 400, 6)
})
future_predictions = model.predict(future_months)
future_df = pd.DataFrame({
    "Future_Month": pd.date_range(df["Month"].max() + pd.offset.MonthBegin(), periods=6, freq="M"),
    "Predict_Revenue": future_predictions
})

print("\n---Future Revenue Forecast---")
print(future_df)

# ---
# 7. Forecast Visualization
# ---
plt.figure(figsize=(10,5))
plt.plot(df["Month"], df["Revenue"], label="Actual Revenue")
plt.plot(future_df["Future_Month"], future_df["Predict_Revenue"], 'r--', label"Forecasted")
plt.title("Revenue Forecast")
plt.legend()
plt_show()

# ---
# 8. Correlation Heatmap
# ---
plt.figure(figsize=(6,4))
sns.heatmap(df.corr(), annot=True, cmap="coolwarm")
plt.title("Feature Correlations")
plt.show()

# ---
# 9. Insight Summary
# ---
print("\n---Insights---")
print("1. Revenue shows steady growth with slight seasonality in Q2 & Q4.")
print("2. Discounts >10% start to reduce profit margins.")
print("3. Linear regression shows strong fit (R² > 0.9).")
print("4. Forecast predicts steady revenue growth for next 6 months.")

# ---
# 10. Export Clean Data & Report 
clean_path = "cleaned_sales_data.xlsx"
df.to_excel(clean_path, index=False)
print(f"\n Clean data exported: {clean_path}")

pdf_report = "sales_forecast_report.pdf"
doc = SimpleDocTemplate(pdf_report, pagesize=letter)
styles = getSampleStyleSheet()
story = [
    Paragraph(" Sales Forecast & Analytics Report", styles["Title']),
    Spacer(1, 12),
    Paragraph(f"Generated on: {datetime.now().strftime('%Y-%m-%d %H:%M')}", styles["Normal"]),
    Spacer(1, 12),
    Paragraph(f"Total Revenue: ₹{df['Revenue'].sum():,.0f}", styles["Normal"]),
    Paragraph(f"Average Profit: ₹{df['Profit'].mean():,.0f}", styles["Normal"]),
    Paragraph("Top Performing Product: " + df.groupby("Product")["Revenue"].sum().idmax(),styles["Normal"]),
    Spacer(1, 12),
    Paragraph("Forecast Summary:", styles["Heading2"]),
    Paragraph(f"Next 6 Months Predicts Total Revenue: ₹ {future_df['Predict_Revenue'].sum():,.0f}, styles["Normal"]),
    Paragraph("The business is projected to see continuous growth based on historical data trends.", styles["Normal"])
]
doc.build(story)
print(f"PDF report created: {pdf_report}")

import pandas as pd

# Employee details
employees = pd.DataFrame({
    'emp_id': [101, 102, 103, 104],
    'first_name': ['Shruti', 'Sanjay', 'Jyoti', 'Nikhil'],
    'last_name': ['Roy', 'Singh', 'Rai', 'Yadav'],
    'dept_id': [1, 2, 1, 3]
})

# Salary Details
salaries  = pd.DataFrame({
    'emp_id': [101, 102, 103, 104],
    'first_name': ['Shruti', 'Sanjay', 'Jyoti', 'Nikhil'],
    'salary': [55000, 65000, 60000, 72000]
})

# Join on multiple colunms: emp_id and first_name
joined_date = pd.merge(employees, salaries, on=['emp_id', 'first_name'], how='inner')
print("Multi-Column Join:\n", joined_data)

# Filter only employees from HR department after joining
departments = pd.DataFrame({
    'dept_id': [1, 2, 3],
    'dept_name': ['HR', 'Finance', 'IT']
})

joined = pd.merge(employees, departments, on='dept_id', how='left')
hr_employeed = joined[joined['dept_name'] == 'HR']
print("\nEmployees From HR Department:\n", hr_employees
# Department data with a different column name
departments_alt = pd.DataFrame({
    'department_id': [1, 2, 3],
    'department_name': ['HR', 'Finance', 'IT']
})

# Joining using Left_on and Right_on
joined_alt = pd.merge(employeed, departments_alt, left_on='dept_id', right_on='department_id', how='left')
print("\n Join Using Different Column Names:\n", joined_alt)

# Product Table
products = pd.DataFrame({
    'product_id': [1, 2, 3],
    'product_name': ['Laptop', 'Mouse', 'Keyboard'],
    'price': [800, 20, 50]
})

# Sales Table
sales = pd.Dataframe({
    'sale_id': [1, 2, 3, 4],
    'product_id': [1, 2, 2, 3],
    'quantity_sold': [5, 10, 3, 7]
})

# Join products and sales
sales_report = pd.merge(products, sales, on='product_id', how='inner')

# Add total sales value
sales_report['total_sales_value'] = sales_report['price'] * sales_report['quantity_sold']

print("\n FINAL SALES REPORT:\n", sales_report)

Import pandas as pd

# Employees details
employees = pd.DataFrame({
    'emp_id': [101, 102, 103, 104],
    'emp_name': ['Shruti', 'Sanjay', 'Jyoti', 'Nikhil'],
    'dept_id': [1, 2, 1, 3]
})

# Department details
departments = pd.DataFrame({
    'dept_id': [1, 2, 3],
    'dept_name': ['HR', 'Finance', 'IT'],
    'location_id': [10, 20, 30]
})

# Location details 
locations = pd.DataFrame({
    'location_id': [10, 20, 30],
    'location_name': ['Prayagraj', 'Ghazipur', 'Varanasi']
})

# Step 1: Join employees with departments
emp_dept = pd.merge(employees, departments, on='dept_id', how='inner')

# Step 2: Join result with locations
final_join = pd.merge(emp_dept, locations, on='location
# Employees data
employees = pd.DataFrame({
    'emp_id': [1, 2, 3, 4, 5],
    'emp_name': ['Amit, 'Riya', 'Sam', 'Neha', 'Raj'],
    'dept_id': [1, 1, 2, 3, 3],
    'salary': [50000, 55000, 60000, 65000, 70000]
})

# Departments data
departments = pd.DataFrame({
    'dept_id': [1, 2, 3],
    'dept_name': ['HR', 'Finance', 'IT']
})

# Join + Aggregate
joined = pd.merge(employees, deprtments, on='dept_id', how='left')

# Group by department and calculate total salary
dept_salary = joined.groupby('dept_name')['salary'].sum().reset_index()

print(" Total Salary Per Department:\n", dept_salary)

# Employees (some with invalid department)
employees = pd.DataFrame({
    'emp_id': [101, 102, 103],
    'emp_name': ['Shruti', 'Sanjay', 'Sukkhi'],
    'dept_id': [1, 4, None]
})

# Departments 
departments = pd.DataFrame({
    'dept_id': [1, 2, 3],
    'dept_name': ['HR', 'Finance', 'IT']
})

# Left join (some employees have no department)
joined = pd.merge(employees, departments, on='dept_id', how='left')

# Fill missing dept_name
joined['dept_name'] = joined['dept_name'].fillna('Not Assigned')

print("Joined With Missing Values Handled:\n", joined)
# Sales Data
sales = pd.DataFrame({
    'sale_id': [1, 2, 3, 4, 5],
    'product_id': [101, 101, 102, 103, 101],
    'quantity': [2, 3, 5, 4, 1]
})

# Product Data
products = pd.DataFrame({
    'product_id': [101, 102, 103],
    'product_name': ['Laptop', 'Mouse' , 'Keyboard'],
    'price': [800, 20, 50]
})

# Join the datasets
report = pd.merge(sales, products, on='product_id', how='left')

# Calculate total sales value
report['total_value'] = report['quantity'] * report['price']

# Summarize
summary = report.groupby('product_name')['total_value'].sum().reset_index()
summary = summary.rename(columns={'total_value': 'Total Sales Value'})

print("PRODUCT SALES SUMMARY:\n", summary)

Imports pandas as pd

# ---
# Step 1: Create Datasets
# ---

# Products data
products = pd.DataFrame({
    'product_id': [1, 2, 3, 4],
    'product_name': ['Laptop', 'Mouse', 'Keyboard', 'Monitor'],
    'category_id': [10, 11, 11, 12],
    'price': [800, 20, 50, 150]
})

# Categories data
categories = pd.DataFrame({
    'category_id': [10, 11, 12],
    'category_name': ['Computers', 'Accessories', 'Displays']
})

# Sales date
sales = pd.DataFrame({
    'sales_id': [101, 102, 103, 104, 105, 106],
    'product_id': [1, 2, 2, 3, 4, 4],
    'quantity': [5, 10, 3, 8, 2, 1],
    'region_id': [201, 202, 202, 203, 201, 204]
})

# Regions data
regions = pd.DataFrame({
    'region_id': [201, 202, 203, 204],
    'region_name': ['North', 'South', 'East', 'West']
})

# ---
# Step 2: Perform Joins
# ---

# Join sales → products
sales_products = pd.merge(sales, products, on='product_id', how='left')

# Join with categories
sales_products = pd.merge(sales_products, categories, on='category_id', how='left')

# Join with regions
final_data = pd.merge(sales_products, regions, on='region_id', how='left')

# ---
# Step 3: Add Calculated Columns
# ---

final_data['total_value'] = final_data['quantity'] * final_data['price']

print("Final Joined Sales Data:\n", final_data)

# ---
# Step 4: Aggregation
# ---

# Total sales by region
region_summary = final_data.groupby('region_name')['total_value'].sum().reset_index()
print("\nSales By Region:\n", region_summary)

# Total sales by category
category_summary = final_final.groupby('category_name')['total_value'].sum().reset_index()
print("\nSales By Category:\n", category_summary)

# Customer and Order Data
customers = pd.DataFrame({
    'cust_id': [1, 2, 3, 4, 5],
    'cust_name': ['Amit', 'Riya', 'Karan', 'Neha', 'Tina']
})

orders = pd.DataFrame({
    'order_id': [101, 102, 103, 104],
    'cust_id': [1, 2, 6, None],
    'amount': [2000, 1500, 3000, 2500]
})

# Join customers and orders
merged = pd.merge(orders, customers, on='cust_id', how='left')

# Handle missing data
merged['cust_name'] = merged['cust_name'].fillna('Unknown Customer')
merged['cust_id'] = merged['cust_id'].fillna(0)

print("Joined & Cleaned Customer Data:\n", merged)

# Employees Performance
performance = df.DataFrame({
    'emp_id': [1, 2, 3, 4],
    'sales_made': [50, 70, 30, 90],
    'targets': [60, 60, 40, 80]
})

# Employee details
employees = pd.DataFames({
    'emp_id': [1, 2, 3, 4],
    'emp_name': ['Amit', 'Riya', 'Sam', 'Neha'],
    'dept': ['Sales', 'Sales', 'HR', 'Sales']
})

# Join
report = pd.merge(employees, performance, on='emp_id', how='inner')

# Calculate achivement %
report['achivement_%'] = round((report['sales_made']/report['targets']) * 100, 2)

# Categorize performance
report['status'] = report['achivement_%'].apply(lambda x: 'Achieved' if x >= 100 else 'Not Achieved')

print("Performance Report:\n", report)
import matplotlib.pyplot as plt

# Reusing the region_summary from example 1
region_summary = pd. DataFrame({
    'region_name': ['North', 'South', 'East', 'West'],
    'total_value': [6400, 260, 400, 300]
})

# Bar Chart of Sales by Region
plt.figure(figsize=(8,5))
plt.bar(region_summary['region_name'], region_summary['total_value'])
plt.title('Total Sales Value by Region')
plt.xlabel('Region')
plt.ylabel('Total Sales Value')
plt.show()

# Customers
customers = pd.DataFrame({
    'cust_id': [1, 2, 3],
    'cust_name': ['Ram', 'Shyam', 'Mahesh']
})

# Orders
orders = pd.DataFrame({
    'order_id': [101, 102, 103, 104],
    'cust_id': [1, 2, 2, 3],
    'order_amount': [500, 700, 800, 400]
})

# Payments
payments = pd.DataFrame({
    'payment_id': [1001, 1002, 1003, 1004],
    'order_id': [101, 102, 104, 104],
    'payment_status': ['Paid', 'Pending', 'Paid', 'Paid']
})

# Join Step 1: Customers → Orders
cust_orders = pd.merge(customers, orders, on='cust_id', how='left')

# Join Step 2: Orders → Payments
final_data = pd.merge(cust_orderd, payments, on='order_id', how='left')

print("Customer Order & Payment Data:\n", final_data)

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8"/>
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>Design Inspiration Gallery</title>
    <link rel="icon" herf="data:;base64,iVBORw0KGg0=">
    <style>
        :root{
            --bg:#0f1724;
            --card:#0b1220;
            --muted:#8a98b0;
            --accent:#8ee7b7;
            --glass: rgba(255,255,255,0.04);
            --radius:14px;
            --gap:18px;
            --max-width:1200px;
            --shadow: 0 6px 20px rgba(2,6,23,0.6);
            font-family: Inter, ui-sans-serif, system-ui,-apple-system,"Segoe UI", Roboto, "Helvetica Neue",
        Arial;
        }
    
    *{box-sizing:border-box}
    html,body{height:100%}
    body{
        margin:0;
        background:
            radial-gradient(1200px 400px at 10%, rgba(110,231,183,0.06), transparent 8%),
            linear-gradient(180deg,#071029 0%, #071323 60%);
        color:#e6eef6;
        -webkit-font-smoothing:antialiased;
        -moz-osx-font-smoothing:grayscale;
        display:flex;
        aling-items:flex-start;
        justify-content:center;
        padding:36px 18px;
    }

    .container{
        width:100%;
        max-width:var(--max-width);
        display:flex;
        flex-direction:column;
        gap:22px;
    }

    header{
        display;flex;
        gap:18px;
        aling-itemms:center;
        justify-content:space-between;
    }

    .title{
        display:flex;
        gap:14px;
        aling-items:center;
    }
    .logo{
        width:56px;height:56px;border-radius:12px;
        background:linear-gradient(135deg,var(--accent),#60a5fa);
        display:grid;place-items:center;font-weight:700;color:#05232a;
        box-shadow: 0 6px 18px rgba(14,38,47,0.3);
        font-family: ui-rounded, system-ui,-apple-system,"Segoe UI",Roboto;
    }
    h1{font-size:20px;margin:0}
    p.lead{margin:0;color:var(--muted);font-size:13px}

    .controls{
        display:flex;
        gap:12px;
        aling-items:center;
        flex-wrap:wrap;
    }

    .search{
        display:flex; gap:8px; align-items:center;
        background:var(--glass); padding:8px 12px; border-radius:10px;
        min-width:220px;
    }
    .search input{
        background:transparent;border:0;color:inherit;outline:0;font-size:14px;width:160px;
    }
    .btn{
        background:linear-gradient(180deg, rgba(255,255,255,0.04), rgba(255,255,255,0.02));
        border:1px solid rgba(255,255,255,0.04);
        color:var(--muted);padding:8px 12px;border-radius:10px;font-size:13px;cursor:pointer;
    }
    .btn.primary{color:#05232a;background:linear-gradient(180deg,var(--accent),#48b3f8);font-weight:600}
    .btn.ghost{background:transparent;border:1px dashed rgba(255,255,255,0,04);color:var(--muted)}

    /*tag filters*/
    .filters{display:flex;gap:8px;flex-wrap:wrap}
    .tag{padding:6px 10px;border-radius:999px;background:rgba(255,255,255,0.02);cursor:pointer;border:1px solid rgba(255,255,255,0.02);font-size:13px;color:var(--muted)}
    .tag.active{background:linear-gradient(90deg,#133b3a,#05232a); color:var(--accent); border-color:rgba(110,231,183,0.14)}
    
    /*gallery*/
    .grid{
        display:grid;
        grid-template-columns: repeat(3, 1fr);
        gap:var(--gap);
    }
    @media (max-width:1000px){.grid{grid-template-columns: repeat(2, 1fr)}}
    @media (max-width:640px){.grid{grid-template-columns: 1fr} .title h1{font-size:18px} }

    .card{
        background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
        border-radius:var(--radius);
        overflow:hidden; display:flex; flex-direction:column;
        box-shadow:var(--shadow); border:1px solid rgba(255,255,255,0.03);
        transition:transform .22s ease, box-shadow .22s ease;
    }
    .card:hover{ transform:translateY(-8px); box-shadow: 0 18px 40px rgba(9,19,29,0.6) }

    .thumb{
        height:180px; width:100%; object-fit:cover; display:block;
        background:linear-gradient(90deg,#0b1220,#071323);
    }

    .card-body{ padding:14px; display:flex; gap:12px; flex-direction:column; flex:1 }
    .meta{display:flex; align-items:center; gap:10px}
    .avatar{width:44px;height:44px;border-radius:10px;background:linear-gradient(135deg,#fff2,#fff1);display:grid;place-items:center;color:#05232a;font-weight:700}
    .card-title{font-size:15px;margin:0;color:#eaf6ff}
    .card-desc{margin:0;color:var(--muted); font-size:13px}

    .badges{display:flex;gap:8px;flex-wrap:wrap}
    .badge{font-size:12px;padding:6px 8px:border-radius:8px;background:rgba(255,255,255,0.02);color:var(--muted);border:1px solid rgba(255,255,255,0.02)}

    .palette{display:flex; gap:6px; margin-top:8px}
    .swatch{width:36px;height:36px;border-radius:8px;border:1px solid rgba(255,255,255,0.06);cursor:pointer;position:relative}
    .swatch span{position:absolute;right:6px;bottom:4px;font-size:10px;color:rgba(255,255,255,0.8);text-shadow:0 1px 2px rgba(0,0,0,0.6)}

    .card-footer{display:flex;justify-content:space-between;align-items:center;gap:10px;margin-top:auto}
    .actions{display:flex;gap:8px;align-items:center}

    /*modal*/
    .modal{
        position:fixed; inset:0; display:none; align-items:center;justify-content:center;
        background:linear-gradient(180deg, rgba(3,6,12,0.5), rgba(3,6,12,0.75));
        padding:36px; z-index:80;
    }
    .modal.open{display:flex}
    .modal-card{
        width:100%; max-width:1000px; border-radius:16px; overflow:hidden; background:var(--card);
        box-shadow:0 30px 80px rgba(2,6,23,0.8); border:1px solid rgba(255,255,255,0.03); padding:36px; z-index:80;
    }
    .modal.open{display:flex}
    .modal-card{
        width:100%; max-width:1000px; border-radius:16px; overflow:hidden; background:var(--card); box-shadow:0 30px 80px rgba(2,6,23,0.8); border:1px solid rgba(255,255,255,0.03);display:grid;grid-template-columns: 1fr 360px;
    }
    @media (max-width:900px){.modal-card{grid-template-columns: 1fr}.modal .meta-right{padding:114px} }

    .modal-hero{height:100%; background-size:cover; background-position:center}
    .meta-right{padding:18px; display:flex;flex-direction:column; gap:12px}
    .close{justify-self:end;border:0;background:transparent;color:var(--muted);cursor:pointer;font-size:20px}

    footer{color:var(--muted);font-size:13px;padding:12px 0;text-align:center}

    /*subtle focus*/
    .tag:focus, .btn:focus, .swatch:focus, .card:focus{outline:2px solid rgba(110,231,183,0.12);outline-offset:3px}

    /*empty state*/
    .empty{padding:48px;border-radius:12px;background:linear-gradient(180deg, rgba(255,255,255,0.02), transparent); text-align:center;color:var(--muted)}
      </style>
</head>
<body>
    <div class="container"role="main"aria-lablledby="main-title">
        <header>
            <div class="logo"aria-hidden="true">DI</div>
            <div>
                <h1 id="main-title">Design Inspiration Gallery</h1>
                <p class="lead">Browse curated UI shots, color palettes & micro-interactions for your next project.</p>
            </div>
        </div>

            <div class="controls" aria-hidden="false">
            <div class="search" role="search">
            <svg width="16" height="16" viewbox="0 0 24 24" fill="none" aria-hidden="true"><path d="M21 21|-4.35-4.35" stroke="currentcolor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"/><circle cx="11" cy="11" r="6" stroke="currentcolor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"/></svg>
            <input id="q" placeholder="Search Keyword, e.g. dashboard" aria-label="Search designs"/>
            </div>
            <div class="filters" role="list" aria-label="Filters" id="filter-list">
            <!--tags injected by JS-->
            </div>
            <button class="btn ghost" id="randomize">Randomize Palette</button>
            <button class="btn primary" id="add-sample">Add Sample</button>
            </div>
        </header>

        <section id="galleryArea" aria-live="polite">
        <div class="grid" id="grid">
        <!--Cards injected by JS-->
        </div>

        <div>
        id="empty" class="empty" style="display:none">
        No results - try different keywords or filters.
        </div>
        </section>
        <footer>
        Tip: click any colour seatch to copy its hex code. Press <kbd>Esc</kbd> to close the preview.
        </footer>
        </div>

        <!-- modal / preview -->
        <div class="modal" id="modal" aria-hidden="true" role="dialog" aria-modal="true">
        <div class="modal-card" role="document">
        <div class="modal-hero" id="modalHero" aria-hidden="true"></div>
        <div class="meta-right">
        <div style="display:flex;gap:12px;align-items:center">
        <div class="avatar" id="modalavtar">D</div>
        <div style="flex:1">
        <div id="modaltitle" style="font-weight:700;fint-size:16px;color:#eaf6ff">Title</div>
        <div id="modalauthor" style="font-size:13px;color:var(--muted)">by Author · <span id="modaltags">tags</span></div>
        </div>
        <button class="close" id="closemodal" aria-label="Close preview">X</button>
        </div>

        <p id="modaldesc" style="color":var(--muted);font-size:13px;color:var(--muted)">Palette</h4>
        <div id="modalPalette" style="display:flex;gap:8px;flex-wrap:wrap"></div>
        </div>

        <div style="margin-top:auto;display:flex;gap:8px">
        <button class="btn" id="visit">Open Image</button>
        <button class="btn" id="copyall">Copy Palette</button>
        </div>
        </div>
        </div>
        </div>

<script>
    /*
    Design Inspiration Gallery - Vanilla JS.
    -Data: samplestory array (id, title, author, img, tags, palette, desc)
    -Features: search, tag filtering, randomize palettes, copy hex
    -Accessibility: keyboard close, focus styles
    */

    const sampleshots = [
        {
            id:'s1', title: 'Finance Dashboard-Night Mode', author:'Lina M.',
            img: 'https://images.unsplash.com/photo-1526378720826-3b2f7b7f6b3e?q=80&w=1600&auto=format&fit=crop&ixlib=rb-4.0.3&s=0a9b6dfd56d6f5b4a4f4d6b9a1d5e9a9',
            tags: ['dashboard','dark','analytics'],
            palette: ['#071323','#0b2b36','#6ee7b7','#60af8ea'],
            desc: 'An elegant analytics dashboard with calm blues & mint highlights for KPIs and charts.'
        },
        {
            id: 's2', title: 'Mobile Onboarding Flow', author:'Akash P.',
            img: 'https://images.unsplash.com/photo-1532619675605-2b9b6b35f8d3?q=80&w=1600&auto=format&fit=crop&ixlib=rb-4.0.3&s=a6d7f8b0c7f4d3b6c9a6f4a0b5e6c1f5',
            tags: ['mobile','ux','illustration'],
            palette: ['#fffaf0','#ffd6a5','#ff8fa3,'#6b7280','#0f1724'],
            desc: 'warm, friendly onboarding with playfull illustrations and soft gradients.'
        },
        {
            id: 's3', title 'E-commerce Product Card', author:'Sana Q.',
            img: 'https://images.unsplash.com/photo-1542291026-7eec264c27ff?q=80&w=1600&auto=format&fit=crop&ixlib=rb-4.0.3&s=cc0a5ef4f3b5f3a963f2b6e6d7c5b6a9',
            tags: ['ecommerce','ui','cards'],
            palettes: ['#ffffff','#0f1724','#f8fafc','#ef4444','#94a3b8'],
            desc: 'Minimal product card concept with clear CTAs and subtle shadows,'
        },
       ];

    cosnt grid = document.getElementByid('Grid');
    cosnt q = document.getElementByid('Q');
    cosnt filterlist = document.getElementByid('Filter-List');
    cosnt emptybox = document.getElementByid('Empty')
    cosnt modal = document.getElementByid('Modal');
    cosnt modalhero = document.getElementByid('ModalHero');
    cosnt modaltitle = document.getElementByid('ModalTitle');
    cosnt modalauthor = document.getElementByid('ModalAuthor');
    cosnt modaltags = document.getElementByid('ModalTags');
    cosnt modaldesc = document.getElementByid('ModalDesc');
    cosnt modalpalette = document.getElementByid('ModalPalette');
    cosnt modalavatar = document.getElementByid('ModalAvatar');
    cosnt closemodal = document.getElementByid('CloseModal');
    cosnt visitbtn = document.getElementByid('Visit');
    cosnt copyallbtn = document.getElementByid('CopyAll');
    cosnt radomizebtn = document.getElementByid('Randomize');
    cosnt addsample btn = document.getElementByid('Add-Sample');

    let activeTags = new set();
    let shots = [...sampleshots];// mutable copy

    //utility
    const $ = s => document.querySelector(s);
    cosnt createNode = (tag, attrs={}, children=[])=>{
        cosnt el = document.creteElement(tag);
        Object.entries(attrs).forEach(([k,v])=>{
            if(k.startsWith('on')) el.addeventlistener(k.slice(2), v);
            else if(k === 'html') el.innerHTML = v;
            else el.setAttribute(k, v);
        });
        children.forEach(c => el.appendChild(c));
        return el;
    }

    function renderFilters(){
        // collect tags
        cosnt tags = Array.from(new Set(shots.flatMap(s => s.tags))).sort();
        filterList.innerHtml =";
            tags.forEach (t=>{
            cosnt btn = createNode('button',{class:'tag',type:'button',tabindex:'0'}.[]);
            btn.textcontent = t;
            btn.addeventlistener('click', () => {
                if(activeTags.has(t)) activeTags.delete(t); else activeTags.add(t);
                btn.classList.toggle('active');
                renderGrid();
            });
            filterList.appendChild(btn);
        });
    }

    function buildCard(shot){
        cosnt card = createNode('article',{class:'card',tabindex:0});
        cosnt thumb = createNode('img',{class:'thumb', src:shot.img,alt: shot.title});
        cosnt body = createNode('div',{class:'card-body'});
        cosnt meta = createNode('div',{class:'meta'});
        cosnt avatar = createNode('div',{class:'avatar',ariaHidden:true},[]); avtar.textContent = shot.author.slice(0,1);
        cosnt title = createNode('h3',{class:'card-title'},[]); title.textcontent = shot.title;
        cosnt desc = createNode('p',{class:'card-desc'},[]); desc.textcontent = shot.desc;
        cosnt badges = createNode('div',{class:'Badges'}); shot.tags.forEach(t=>{ cosnt b = createNode('span',{class:'Badges'}); b.textcontent = t; badges.appendChild(b) });
        cosnt palette = createNode('div',{class:'palette'}); shot.palette.forEach(hex=>{
            cosnt sw = createNode('button',{class:'swatch',type:'button','aria-label':Copy ${hex} ⁠});
        sw.style.background = hex;
        sw.title = Copy ${hex} ⁠;
        sw.addEventlistener('click', (e)=>{ navigator.clipboard.writetext(hex).then(()=>{ sw.animate([{transform:'scale(1)'},{transform:'scale(0.96)'},{transform:'scale(1)'}],{duration;220}) }) });
    palette.appendChild(sw);
    });

    meta.appendChild(avtar);
    meta.appendChild(createNode('div',{html:⁠<div style="font-weight:700">${shot.author}</div><div style="color:var(--muted);font-size:13px">${shot.tags.join(' • ')}</div> ⁠}));
    body.appendChild(meta);
    body.appendChild(title);
    body.appendChild(desc);
    body.appendChild(badges);
    body.appendChild(palette);

    cosnt footer = createNode('div',{class:'card-footer'});
    cosnt action = createNode('div',{class:'actions'});
    cosnt viewBtn = createNode('button',{class:'btn',type:'button'},[]); viewbtn.textcontent = 'Preview'; viewbtn.addEventListener('click', ()=> openModal(shot));action.appendChild(viewBtn);
    cosnt copyBtn = cretaNode('button',{class:'btn ghost',type:'button'},[]); copyBtn.textContent='copy palette'; copyBtn.addEventListener('click', ()=>{
        navigator.clipboard.writeText(shot.palette.join(', ')); copyBtn.animate([{opacity:1},{opacity:0.6},{opacity:1}],{duration:300});
    });

    footer.appendChild(actions);
    footer.appendChild(copyBtn);

    body.appendChild(footer);

    card.appendChild(thumb);
    card.appendChild(body);

    //keyboard: Enter opens preview
    card.addEventListener('keydown', e=>{
        if(e.key ==='Enter') openModal(shot);
    });

    return card;
    }

    

    

</script>
</body>
