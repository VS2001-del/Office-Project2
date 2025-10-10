EMPLOYEE MANAGEMENT SYSTEM

CREATE TABLE employee(
    emp_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    department VARCHAR(100),
    salary FLOAT
);

Database Connection
con=mysql.connector.connect(
    host="localhost",
    password="root",
    database="employee_db"
)

cursor=con.cursor()

Add new employee
def add_employee():
    name=input("enter employee name:")
    department=input(enter department:")
    salary=float(input("enter salary:"))
    query="INSERT INTO employee (name, department, salary)"
            VALUES (%s,%s,%s)
    cursor.execute(query,(name, department, salary))
    con.commit()
    Print("Employee Added Successfully!")

View All Employee
def view_employee():
    cursor.execute("Select * From employee")
    row= cursor.fetchall()
    print("\n--Employee Records--")
    for row in rows:
    print(f"ID:{row[0]}, name:{[1]}, Dept: {row[2]}, salary: {[3]}")
    Print("------")

Search Employee
def search_employee():
    emp_id= input("Enter Employee ID To Search:")
    cursor.execute("Select * From Employee WHERE emp_id = %s"'(emp_id,))
    row = cursor.fetchone()
    if row:
    Print(f"\nID: {row[0]], name: {row[1]}, Dept: {row[2]}, salary: {row[3]}")
    else:
    Print("Employee Not Found")


Update Employee
def update_employee():
    emp_id = input("Enter Employee ID To Update:")
    new_salary = float(input("Enter new salary:"))
    query = "UPDATE employees Set salary = %s WHERE emp_id=%s"
    cursor.execute(query,(new_salary, emp_id))
    con.commit()
    Print("Employee Updated Successfully")


Delete Employee
def delete_employee():
    emp_id=input("Enter Employee ID To Delete:")
    cursor.execute("DELETE FROM employee WHERE emp_id=%s",(emp_id,))
    con.commit()
    Print("Employee Deleted Successfully")


Menu
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
