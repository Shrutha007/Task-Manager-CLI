import json
import os

TASKS_FILE = "tasks.json"

# Load existing tasks
def load_tasks():
    if os.path.exists(TASKS_FILE):
        with open(TASKS_FILE, "r") as file:
            return json.load(file)
    return []

# Save tasks to file
def save_tasks(tasks):
    with open(TASKS_FILE, "w") as file:
        json.dump(tasks, file, indent=4)

# Add a new task
def add_task(description, priority):
    tasks = load_tasks()
    tasks.append({"description": description, "priority": priority, "completed": False})
    save_tasks(tasks)
    print("Task added successfully!")

# List all tasks
def list_tasks():
    tasks = load_tasks()
    if not tasks:
        print("No tasks found!")
    else:
        for idx, task in enumerate(tasks, 1):
            status = "✅" if task["completed"] else "❌"
            print(f"{idx}. {task['description']} [Priority: {task['priority']}] {status}")

# Mark task as completed
def complete_task(task_number):
    tasks = load_tasks()
    if 1 <= task_number <= len(tasks):
        tasks[task_number - 1]["completed"] = True
        save_tasks(tasks)
        print("Task marked as completed!")
    else:
        print("Invalid task number!")

# Delete a task
def delete_task(task_number):
    tasks = load_tasks()
    if 1 <= task_number <= len(tasks):
        del tasks[task_number - 1]
        save_tasks(tasks)
        print("Task deleted successfully!")
    else:
        print("Invalid task number!")

# Main menu
def main():
    while True:
        print("\nTask Manager CLI")
        print("1. Add Task")
        print("2. List Tasks")
        print("3. Complete Task")
        print("4. Delete Task")
        print("5. Exit")
        choice = input("Enter your choice: ")

        if choice == "1":
            description = input("Enter task description: ")
            priority = input("Enter task priority (High/Medium/Low): ")
            add_task(description, priority)
        elif choice == "2":
            list_tasks()
        elif choice == "3":
            list_tasks()
            task_number = int(input("Enter task number to complete: "))
            complete_task(task_number)
        elif choice == "4":
            list_tasks()
            task_number = int(input("Enter task number to delete: "))
            delete_task(task_number)
        elif choice == "5":
            print("Exiting Task Manager. Goodbye!")
            break
        else:
            print("Invalid choice! Please try again.")

if __name__ == "__main__":
    main()
