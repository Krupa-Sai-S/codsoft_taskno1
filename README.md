# codsoft_taskno1
A To-Do List application is a useful project that helps users manage and organize their tasks efficiently. This project aims to create a command-line or GUI-based application using Python, allowing  users to create, update, and track their to-do lists
# Define a Task class
class Task:
    def __init__(self, title, description, due_date, status="pending"):
        self.title = title
        self.description = description
        self.due_date = due_date
        self.status = status

    def __str__(self):
        return f"Title: {self.title}\nDescription: {self.description}\nDue Date: {self.due_date}\nStatus: {self.status}\n"

# Define a ToDoList class with file handling
import json

class ToDoList:
    def __init__(self):
        self.tasks = []

    def add_task(self, task):
        self.tasks.append(task)

    def view_tasks(self):
        if not self.tasks:
            print("No tasks available.")
        for idx, task in enumerate(self.tasks, start=1):
            print(f"Task {idx}:\n{task}")

    def update_task(self, index, title=None, description=None, due_date=None, status=None):
        if 0 <= index < len(self.tasks):
            task = self.tasks[index]
            if title:
                task.title = title
            if description:
                task.description = description
            if due_date:
                task.due_date = due_date
            if status:
                task.status = status
        else:
            print("Invalid task index.")

    def delete_task(self, index):
        if 0 <= index < len(self.tasks):
            del self.tasks[index]
        else:
            print("Invalid task index.")

    def complete_task(self, index):
        if 0 <= index < len(self.tasks):
            self.tasks[index].status = "completed"
        else:
            print("Invalid task index.")

    def save_to_file(self, filename):
        tasks_data = [task.__dict__ for task in self.tasks]
        with open(filename, 'w') as file:
            json.dump(tasks_data, file)

    def load_from_file(self, filename):
        try:
            with open(filename, 'r') as file:
                tasks_data = json.load(file)
                self.tasks = [Task(**data) for data in tasks_data]
        except FileNotFoundError:
            print("File not found.")
        except json.JSONDecodeError:
            print("Error decoding JSON.")

# Define the main CLI function
def main():
    todo_list = ToDoList()

    while True:
        print("\nTo-Do List Application")
        print("1. Add Task")
        print("2. View Tasks")
        print("3. Update Task")
        print("4. Delete Task")
        print("5. Complete Task")
        print("6. Save Tasks")
        print("7. Load Tasks")
        print("8. Exit")

        choice = input("Choose an option: ")

        if choice == '1':
            title = input("Enter task title: ")
            description = input("Enter task description: ")
            due_date = input("Enter task due date: ")
            task = Task(title, description, due_date)
            todo_list.add_task(task)
        elif choice == '2':
            todo_list.view_tasks()
        elif choice == '3':
            index = int(input("Enter task index to update: ")) - 1
            title = input("Enter new title (leave blank to keep current): ")
            description = input("Enter new description (leave blank to keep current): ")
            due_date = input("Enter new due date (leave blank to keep current): ")
            status = input("Enter new status (leave blank to keep current): ")
            todo_list.update_task(index, title, description, due_date, status)
        elif choice == '4':
            index = int(input("Enter task index to delete: ")) - 1
            todo_list.delete_task(index)
        elif choice == '5':
            index = int(input("Enter task index to complete: ")) - 1
            todo_list.complete_task(index)
        elif choice == '6':
            filename = input("Enter filename to save tasks: ")
            todo_list.save_to_file(filename)
        elif choice == '7':
            filename = input("Enter filename to load tasks: ")
            todo_list.load_from_file(filename)
        elif choice == '8':
            break
        else:
            print("Invalid choice. Please try again.")

main()
