import json
import os
from datetime import datetime

data_file = "daily_reports.txt"

def load_reports():
    if os.path.exists(data_file):
        with open(data_file, "r") as file:
            return json.load(file)
    return []

def save_reports(reports):
    with open(data_file, "w") as file:
        json.dump(reports, file, indent=4)

def add_report():
    date = datetime.today().strftime('%Y-%m-%d')
    tasks = input("What I did today: ")
    challenges = input("Challenges faced: ")
    remark = input("Today's Remark: ")
    
    reports = load_reports()
    reports.append({"date": date, "tasks": tasks, "challenges": challenges, "remark": remark})
    save_reports(reports)
    print("Report added successfully!")

def view_reports():
    reports = load_reports()
    if not reports:
        print("No reports found.")
        return
    for i, report in enumerate(reports, 1):
        print(f"\nReport {i} - Date: {report['date']}")
        print(f"Tasks: {report['tasks']}")
        print(f"Challenges: {report['challenges']}")
        print(f"Remark: {report['remark']}")
        print("-" * 40)

def edit_report():
    reports = load_reports()
    view_reports()
    index = int(input("Enter report number to edit: ")) - 1
    if 0 <= index < len(reports):
        reports[index]['tasks'] = input("New Tasks: ") or reports[index]['tasks']
        reports[index]['challenges'] = input("New Challenges: ") or reports[index]['challenges']
        reports[index]['remark'] = input("New Remark: ") or reports[index]['remark']
        save_reports(reports)
        print("Report updated successfully!")
    else:
        print("Invalid report number.")

def delete_report():
    reports = load_reports()
    view_reports()
    index = int(input("Enter report number to delete: ")) - 1
    if 0 <= index < len(reports):
        del reports[index]
        save_reports(reports)
        print("Report deleted successfully!")
    else:
        print("Invalid report number.")

def export_reports():
    reports = load_reports()
    if not reports:
        print("No reports to export.")
        return
    filename = f"reports_{datetime.today().strftime('%Y-%m-%d')}.txt"
    with open(filename, "w") as file:
        for report in reports:
            file.write(f"Date: {report['date']}\n")
            file.write(f"Tasks: {report['tasks']}\n")
            file.write(f"Challenges: {report['challenges']}\n")
            file.write(f"Remark: {report['remark']}\n")
            file.write("-" * 40 + "\n")
    print(f"Reports exported to {filename}")

def main():
    while True:
        print("\nDaily Report Tracker")
        print("1. Add Report")
        print("2. View Reports")
        print("3. Edit Report")
        print("4. Delete Report")
        print("5. Export Reports")
        print("6. Exit")
        
        choice = input("Enter your choice: ")
        if choice == "1":
            add_report()
        elif choice == "2":
            view_reports()
        elif choice == "3":
            edit_report()
        elif choice == "4":
            delete_report()
        elif choice == "5":
            export_reports()
        elif choice == "6":
            print("Exiting the program.")
            break
        else:
            print("Invalid choice, please try again.")

if __name__ == "__main__":
    main()
