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
    VALUES ('Engineering','Dehradun');

INSERT INTO Manager (first_name, last_name, email_id, dept_id)
        VALUES ('Sankalp', 'Srivastava', 'srivastavasankalpos28@gmail.com', 1);

INSERT INTO Project (title, description, manager_id, start_date, end_date, status)
        VALUES ('DB Optimization','Optimize queries and schema', '1', '2025-08-01', '2025-11-09', ongoing);

INSERT INTO Intern (first_name, last_name, email_id, university, start_date, dept_id)
        VALUES ('Vyom','Singh','officialvyomsingh2001@gmail.com, RKGIT, '2025-08-01',1);

INSERT INTO Intern_Project (intern_id, Project_id, role, weekly_hours)
        VALUES (1,1,'DB Developer', 40);

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

--IT INTERNSHIP PROJECT MANAGEMENT SYSTEM DATABASE
--

--DROP OLD TABLES IF THEY EXIST (Safe re-run)
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
    
