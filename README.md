# Student-Management-System
The Student Information System is a Python-based app that manages student data, including adding students, generating report cards, tracking attendance, and analyzing subject performance. It features CSV export, parent updates, and student ranking, making it easy for schools to manage student records and performance.
import csv

# Initialize an empty dictionary to store student details
students = {}

# Function to add a new student
def add_student():
    name = input("Enter student's name: ")
    age = int(input(f"Enter {name}'s age: "))
    student_class = input(f"Enter {name}'s class: ")
    grades = input(f"Enter {name}'s grades (separated by space): ")
    subjects = {}
    subjects_input = input(f"Enter {name}'s subjects and grades (subject:grade format, comma separated): ")
    
    # Split subjects and grades
    for sub in subjects_input.split(','):
        subject, grade = sub.split(':')
        subjects[subject.strip()] = int(grade.strip())
    
    contact_email = input(f"Enter {name}'s parent's email: ")
    students[name] = {'age': age, 'class': student_class, 'grades': grades, 'subjects': subjects, 'contact': {'email': contact_email}}
    print(f"{name} added successfully.\n")

# Function to generate report card for a student
def generate_report_card():
    name = input("Enter the student's name to generate report card: ")
    if name in students:
        grades = list(map(int, students[name]['grades'].split()))
        total = sum(grades)
        avg = total / len(grades)
        print(f"\nReport Card for {name}:")
        print(f"Total Marks: {total}")
        print(f"Average Marks: {avg:.2f}")
        if avg >= 90:
            grade = 'A'
        elif avg >= 75:
            grade = 'B'
        elif avg >= 50:
            grade = 'C'
        else:
            grade = 'F'
        print(f"Overall Grade: {grade}\n")
    else:
        print("Student not found.\n")

# Function to mark attendance for a student
def mark_attendance():
    name = input("Enter the student's name to mark attendance: ")
    if name in students:
        present = input("Is the student present today? (y/n): ").lower() == 'y'
        if 'attendance' not in students[name]:
            students[name]['attendance'] = []
        students[name]['attendance'].append(present)
        print(f"Attendance marked for {name}.\n")
    else:
        print("Student not found.\n")

# Function to view attendance records of a student
def view_attendance():
    name = input("Enter the student's name to view attendance: ")
    if name in students and 'attendance' in students[name]:
        attendance = students[name]['attendance']
        attended_days = attendance.count(True)
        total_days = len(attendance)
        print(f"{name} has attended {attended_days}/{total_days} days. Attendance percentage: {attended_days / total_days * 100:.2f}%\n")
    else:
        print("No attendance records found for this student.\n")

# Function to analyze student's performance subject-wise
def subject_wise_analysis():
    name = input("Enter student's name: ")
    if name in students and 'subjects' in students[name]:
        subjects = students[name]['subjects']
        best_subject = max(subjects, key=subjects.get)
        worst_subject = min(subjects, key=subjects.get)
        print(f"Best subject for {name}: {best_subject} ({subjects[best_subject]})")
        print(f"Needs improvement in: {worst_subject} ({subjects[worst_subject]})\n")
    else:
        print("Student not found or no subject data available.\n")

# Function to rank students based on average grades
def rank_students():
    ranking = sorted(students.items(), key=lambda x: sum(map(int, x[1]['grades'].split())) / len(x[1]['grades'].split()), reverse=True)
    print("\nStudent Rankings:")
    for rank, (name, details) in enumerate(ranking, start=1):
        avg = sum(map(int, details['grades'].split())) / len(details['grades'].split())
        print(f"{rank}. {name} - Average Marks: {avg:.2f}")
    print()

# Function to simulate sending update to parents
def send_update_to_parents():
    name = input("Enter student's name to send update: ")
    if name in students and 'contact' in students[name]:
        contact = students[name]['contact']
        print(f"Sending update to {name}'s parent at {contact['email']}:\n")
        print(f"Dear Parent, {name}'s current attendance is excellent. Keep up the good work!\n")
    else:
        print("No contact details found.\n")

# Function to export student data to a CSV file
def export_to_csv():
    with open('students.csv', 'w', newline='') as file:
        writer = csv.writer(file)
        writer.writerow(['Name', 'Age', 'Class', 'Grades', 'Subjects', 'Parent Email'])
        for name, details in students.items():
            writer.writerow([name, details['age'], details['class'], details['grades'], details['subjects'], details['contact']['email']])
    print("Student data exported to students.csv\n")

# Main menu
def main():
    while True:
        print("=== Student Information System ===")
        print("1. Add Student")
        print("2. Generate Report Card")
        print("3. Mark Attendance")
        print("4. View Attendance")
        print("5. Subject-wise Analysis")
        print("6. Rank Students")
        print("7. Send Update to Parents")
        print("8. Export Data to CSV")
        print("9. Exit")
        choice = input("Enter your choice: ")

        if choice == '1':
            add_student()
        elif choice == '2':
            generate_report_card()
        elif choice == '3':
            mark_attendance()
        elif choice == '4':
            view_attendance()
        elif choice == '5':
            subject_wise_analysis()
        elif choice == '6':
            rank_students()
        elif choice == '7':
            send_update_to_parents()
        elif choice == '8':
            export_to_csv()
        elif choice == '9':
            print("Exiting the system.")
            break
        else:
            print("Invalid choice. Please try again.\n")

# Run the main menu
main()
