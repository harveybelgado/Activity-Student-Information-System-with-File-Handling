Student-Information-System-with-File-Handling

this is a file-based Student Information System, a program designed to manage student records using a file for permanent data storage. It offers core functionalities to add, update, and delete student records, while also allowing you to display a list of all students. The program's ability to read and write data ensures that information is not lost when the application is closed. Furthermore, it is built with specific error handling to provide a smooth user experience by preventing issues like duplicate student IDs, invalid user input, or problems with file access.


<img width="1360" height="768" alt="Screenshot (414)" src="https://github.com/user-attachments/assets/6258c7d1-3b09-4269-96a5-3222548d3c40" />

<img width="1360" height="768" alt="Screenshot (415)" src="https://github.com/user-attachments/assets/fbb68669-e2e6-4a2a-9809-0b68ddfa84a7" />

<img width="1360" height="768" alt="Screenshot (416)" src="https://github.com/user-attachments/assets/653c6193-ce0a-45ae-b512-68a78b4e519e" />

<img width="1360" height="768" alt="Screenshot (417)" src="https://github.com/user-attachments/assets/7b00e865-a5d6-4d3d-8528-97dbdbe7d2c1" />

<img width="1360" height="768" alt="Screenshot (418)" src="https://github.com/user-attachments/assets/4d50f8be-bd7d-41cb-8f11-f1d9d926c8d0" />

My primary struggle is data management and persistence, as it requires implementing the logic to save the student records from memory to a file and correctly load them back, while handling potential file-related errors like a FileNotFoundError. Another significant hurdle is building a robust program flow and user interface, which involves creating a continuous loop for the main menu and ensuring the program responds correctly to user selections. Finally, making the system reliable requires thorough error handling to validate user input, prevent duplicate student IDs, and manage instances where a student record isn't found, ensuring the program doesn't crash from common mistakes.



[SIS.txt](https://github.com/user-attachments/files/22175077/SIS.txt)
|09921|Harvey|18|90, 89,
|09222|Jisa|18|99, 99, 99

[SIS_FileHandling.py](https://github.com/user-attachments/files/22175083/SIS_FileHandling.py)
import os

SIS_students = {} # dictionary to store
FILE_NAME = "SIS.txt" # to save and load data

# functions
def add_stu():
    """ Add a new student to the system """
    try:
        stu_id = input("Enter Student ID: ")

        if stu_id in SIS_students:
            print("Error 404: Student ID is already registered")
            return

        stu_name = input("Enter Student Name: ")

        # Handle invalid age input
        try:
            stu_age = int(input("Enter Student Age: "))
        except ValueError:
            print("Invalid age! Please try again.")
            return

        # Enter multiple grades
        stu_grade = input("Enter Student Grade separated by space: ").split()
        try:
            grades = [int(g) for g in stu_grade]
        except ValueError:
            print("Invalid grade! Please try again.")
            return

        SIS_students[stu_id] = {
            "ID": stu_id,
            "Name":stu_name,
            "Age": stu_age,
            "Grade": grades,
            "Status": True
        }
        print(" Student added Successfully! ")

    except Exception as e:
        print(f"Error adding student: {e}")

def display_stu():
    """ Display the students registered """
    try:
        if not SIS_students:
            print("No students registered")
            return

        for stu_id, details in SIS_students.items():
            print(f"\nStudent ID: {stu_id}")
            print(f"Name: {details['Name'][1]}")
            print(f"Age: {details['Age']}")

            print(f"Grade: ", end="")
            for grade in details['Grade']:
                print(grade, end=", ")
                print()

                # Lambda for avg scale grade
                sis_avg = (lambda g: sum(g) / len(g) if g else 0)(details['Grade'])
                print(f"Average Grade: {sis_avg:.2f}")

    except Exception as e:
        print(f"Error displaying student: {e}")

def update_stu():
    """ Update the students registered """
    try:
        stu_id = input("Enter Student ID to update: ")

        if stu_id not in SIS_students:
            print("Error 404: Student ID not found!")
            return

        print("Leave blank to update student's grade")

        stu_name = input("Enter Student Name: ")
        stu_age = input("Enter Student Age: ")
        stu_grade = input("Enter Student Grade separated by space: ")

        if stu_name:
            SIS_students[stu_id]["Name"] = stu_name

        if stu_age:
            try:
                SIS_students[stu_id]["Age"] = int(stu_age)
            except ValueError:
                print("Invalid age! Please try again.")

        if stu_grade:
            try:
                SIS_students[stu_id]["Grade"] = [int(g) for g in stu_grade.split()]
            except ValueError:
                print("Invalid grade! Please try again.")

        print("Student updated successfully!")

    except Exception as e:
        print(f"Error updating student: {e}")

def delete_stu():
    """ Delete the students registered """
    try:
        stu_id = input("Enter Student ID to delete: ")

        if stu_id not in SIS_students:
            del SIS_students[stu_id]
            print("Student deleted successfully!")

        else:
            print("Error 404: Student ID not found!")
    except Exception as e:
        print(f"Error deleting student: {e}")


def save_stu():
    """ Save the students registered to the file """
    try:
        with open(FILE_NAME, "w") as f:
            for stu_id, details in SIS_students.items():
                line = "f{stu_id},{details['ID'][1]},{details['Name'][1]}, {details[stu_age]}, {' '.join(map(str, details['Grade']))},{details['Status']}}\n"
                f.write(line)

        print(f"Data saved successfully!")
    except Exception as e:
        print(f"Error saving file: {e}")

def load_stu():
    """ Load the students registered to the file """
    try:
        if not os.path.exists(FILE_NAME):
            return

        with open(FILE_NAME, "r") as f:
            for line in f:
                parts = line.strip().split(",")
                if len(parts) < 4:
                    continue

                stu_id = parts[0]
                stu_name = parts[1]
                try:
                    stu_age = int(parts[2])
                except ValueError:
                    stu_age = 0 # default invalid if

                try:
                    stu_grade = list(map(int, parts[3].split()))
                except ValueError:
                    stu_grade = []

                SIS_students[stu_id] = {
                    "ID": stu_id,
                    "Name": stu_name,
                    "Age": stu_age,
                    "Grade": stu_grade,
                    "Status": True
                }

        print("Data loaded successfully!")
    except Exception as e:
        print(f"Error loading file: {e}")

# Main Program
def sis_main():
    load_stu()

    while True:
        print("--------------Welcome to Student Information System File Handling <3-------------")
        print("1. Add Student")
        print("2. Update Student")
        print("3. Delete Student")
        print("4. Save Student")
        print("5. Save to File")
        print("6. Load from File")
        print("7. Exit")

        choose = input("\nEnter your selection: ")

        try:
            if choose == "1":
                add_stu()
            elif choose == "2":
                update_stu()
            elif choose == "3":
                delete_stu()
            elif choose == "4":
                save_stu()
            elif choose == "5":
                load_stu()
            elif choose == "6":
                delete_stu()
            elif choose == "7":
                print("---------Thank you for using Student Information System File Handling <3------")
                break

            else:
                print("Invalid selection! Please try again.")

        except Exception as e:
            print(f"An unexpected error occurred: {e}")

if __name__ == "__main__":
    sis_main()














