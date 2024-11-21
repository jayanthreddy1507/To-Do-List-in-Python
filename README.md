import json
import os

class TodoApp:
    def __init__(self, filename='todos.json'):
        self.filename = filename
        self.load_tasks()

    def load_tasks(self):
        if os.path.exists(self.filename):
            with open(self.filename, 'r') as file:
                self.tasks = json.load(file)
        else:
            self.tasks = []

    def save_tasks(self):
        with open(self.filename, 'w') as file:
            json.dump(self.tasks, file, indent=4)

    def add_task(self, title, description):
        task = {
            'title': title,
            'description': description,
            'completed': False
        }
        self.tasks.append(task)
        self.save_tasks()
        print(f'Task "{title}" added.')

    def view_tasks(self):
        if not self.tasks:
            print("No tasks found.")
            return
        for index, task in enumerate(self.tasks, start=1):
            status = '✓' if task['completed'] else '✗'
            print(f"{index}. [{status}] {task['title']} - {task['description']}")

    def update_task(self, index, title=None, description=None, completed=None):
        if 0 <= index < len(self.tasks):
            if title is not None:
                self.tasks[index]['title'] = title
            if description is not None:
                self.tasks[index]['description'] = description
            if completed is not None:
                self.tasks[index]['completed'] = completed
            self.save_tasks()
            print(f'Task {index + 1} updated.')
        else:
            print("Task not found.")

    def delete_task(self, index):
        if 0 <= index < len(self.tasks):
            removed_task = self.tasks.pop(index)
            self.save_tasks()
            print(f'Task "{removed_task["title"]}" deleted.')
        else:
            print("Task not found.")

def main():
    app = TodoApp()
    while True:
        print("\nTo-Do List Application")
        print("1. Add Task")
        print("2. View Tasks")
        print("3. Update Task")
        print("4. Delete Task")
        print("5. Exit")
        
        choice = input("Choose an option: ")
        
        if choice == '1':
            title = input("Enter task title: ")
            description = input("Enter task description: ")
            app.add_task(title, description)
        elif choice == '2':
            app.view_tasks()
        elif choice == '3':
            index = int(input("Enter task number to update: ")) - 1
            title = input("Enter new title (leave blank to keep current): ")
            description = input("Enter new description (leave blank to keep current): ")
            completed = input("Mark as completed? (y/n): ").lower() == 'y'
            app.update_task(index, title if title else None, description if description else None, completed)
        elif choice == '4':
            index = int(input("Enter task number to delete: ")) - 1
            app.delete_task(index)
        elif choice == '5':
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
