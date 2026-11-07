import json
from pathlib import Path

FILE = Path("tasks.json")

def load_tasks():
    if FILE.exists():
        return json.loads(FILE.read_text())
    return []

def save_tasks(tasks):
    FILE.write_text(json.dumps(tasks, indent=4))

def show_tasks(tasks):
    if not tasks:
        print("No tasks found.")
        return

    for i, task in enumerate(tasks, 1):
        status = "Done" if task["done"] else "Pending"
        print(f"{i}. {task['name']} - {status}")

def main():
    tasks = load_tasks()

    while True:
        print("\n=== Python Task Manager ===")
        print("1. Add task")
        print("2. View tasks")
        print("3. Complete task")
        print("4. Delete task")
        print("5. Exit")

        choice = input("Choose an option: ")

        if choice == "1":
            name = input("Task name: ").strip()
            if name:
                tasks.append({"name": name, "done": False})
                save_tasks(tasks)
                print("Task added!")

        elif choice == "2":
            show_tasks(tasks)

        elif choice == "3":
            show_tasks(tasks)
            try:
                index = int(input("Task number: ")) - 1
                if 0 <= index < len(tasks):
                    tasks[index]["done"] = True
                    save_tasks(tasks)
                    print("Task completed!")
                else:
                    print("Invalid task number.")
            except ValueError:
                print("Enter a valid number.")

        elif choice == "4":
            show_tasks(tasks)
            try:
                index = int(input("Task number: ")) - 1
                if 0 <= index < len(tasks):
                    tasks.pop(index)
                    save_tasks(tasks)
                    print("Task deleted!")
                else:
                    print("Invalid task number.")
            except ValueError:
                print("Enter a valid number.")

        elif choice == "5":
            print("Goodbye!")
            break

        else:
            print("Invalid option.")

if __name__ == "__main__":
    main()
