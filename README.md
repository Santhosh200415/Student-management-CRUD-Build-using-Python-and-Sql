# Student-management-CRUD-Build-using-Python-and-Sql
CRUD Functions:  create_student: Adds a new student record to the database.read_students: Fetches and prints all student records.update_student: Updates a student's details based on the given ID.delete_student: Removes a student record using the ID. 

import sqlite3

# Connect to SQLite database (or create it if it doesn't exist)
conn = sqlite3.connect('student_management.db')

# Create a cursor object to execute SQL commands
cur = conn.cursor()

# Create table
cur.execute('''CREATE TABLE IF NOT EXISTS students (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                name TEXT NOT NULL,
                age INTEGER NOT NULL,
                grade TEXT NOT NULL
              )''')
conn.commit()

# Function to add a student
def create_student(name, age, grade):
    cur.execute("INSERT INTO students (name, age, grade) VALUES (?, ?, ?)", (name, age, grade))
    conn.commit()
    print("Student added successfully!")

# Function to read all students
def read_students():
    cur.execute("SELECT * FROM students")
    rows = cur.fetchall()
    for row in rows:
        print(f"ID: {row[0]}, Name: {row[1]}, Age: {row[2]}, Grade: {row[3]}")

# Function to update a student's record
def update_student(student_id, name=None, age=None, grade=None):
    cur.execute("SELECT * FROM students WHERE id = ?", (student_id,))
    student = cur.fetchone()
    if not student:
        print("Student not found!")
        return

    # Update the fields if they are provided
    new_name = name if name else student[1]
    new_age = age if age else student[2]
    new_grade = grade if grade else student[3]

    cur.execute("UPDATE students SET name = ?, age = ?, grade = ? WHERE id = ?", (new_name, new_age, new_grade, student_id))
    conn.commit()
    print("Student updated successfully!")

# Function to delete a student
def delete_student(student_id):
    cur.execute("DELETE FROM students WHERE id = ?", (student_id,))
    conn.commit()
    print("Student deleted successfully!")

# Main program to test the CRUD functions
if __name__ == "__main__":
    while True:
        print("\n--- Student Management ---")
        print("1. Add Student")
        print("2. View Students")
        print("3. Update Student")
        print("4. Delete Student")
        print("5. Exit")

        choice = input("Enter your choice: ")

        if choice == '1':
            name = input("Enter student's name: ")
            age = int(input("Enter student's age: "))
            grade = input("Enter student's grade: ")
            create_student(name, age, grade)
        elif choice == '2':
            print("\n--- Student List ---")
            read_students()
        elif choice == '3':
            student_id = int(input("Enter student's ID to update: "))
            name = input("Enter new name (leave blank to keep unchanged): ")
            age = input("Enter new age (leave blank to keep unchanged): ")
            grade = input("Enter new grade (leave blank to keep unchanged): ")
            update_student(student_id, name if name else None, int(age) if age else None, grade if grade else None)
        elif choice == '4':
            student_id = int(input("Enter student's ID to delete: "))
            delete_student(student_id)
        elif choice == '5':
            break
        else:
            print("Invalid choice! Please try again.")
