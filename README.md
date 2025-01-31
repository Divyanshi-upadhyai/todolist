# todolist
to create a to do list using python
import csv
import os
import tkinter as tk
from tkinter import messagebox, simpledialog

def read_tasks(file_name='tasks.csv'):
    tasks = []
    if os.path.exists(file_name):
        with open(file_name, newline='') as csvfile:
            reader = csv.reader(csvfile)
            tasks = list(reader)
    return tasks

def write_tasks(tasks, file_name='tasks.csv'):
    with open(file_name, 'w', newline='') as csvfile:
        writer = csv.writer(csvfile)
        writer.writerows(tasks)

def add_task():
    task = simpledialog.askstring("Add Task", "Enter a new task:")
    if task:
        tasks.append([task, False])
        write_tasks(tasks)
        update_task_list()
    else:
        messagebox.showwarning("Input Error", "Task cannot be empty.")

def remove_task():
    selected_task_indices = task_listbox.curselection()
    if selected_task_indices:
        for index in selected_task_indices[::-1]:
            tasks.pop(index)
        write_tasks(tasks)
        update_task_list()
    else:
        messagebox.showwarning("Selection Error", "Please select a task to remove.")

def ChangeTask_task_status():
    selected_task_indices = task_listbox.curselection()
    for index in selected_task_indices:
        tasks[index][1] = not tasks[index][1]
    write_tasks(tasks)
    update_task_list()

def update_task_list():
    task_listbox.delete(0, tk.END)
    for task in tasks:
        status = '✓' if task[1] else '✗'
        task_listbox.insert(tk.END, f"{task[0]} [{status}]")

def create_gui():
    global task_listbox
    root = tk.Tk()
    root.title("To-Do List Application")
    root.configure(bg='lavender')

    header = tk.Label(root, text="To-Do List Application", bg='lavender', fg='black', font=('Arial', 16))
    header.pack(pady=10)

    task_listbox = tk.Listbox(root, width=50, height=10, bg='lavenderblush', fg='black', font=('Arial', 12))
    task_listbox.pack(pady=10)

    button_frame = tk.Frame(root, bg='lavender')
    button_frame.pack(pady=10)

    add_button = tk.Button(button_frame, text="Add Task", command=add_task, bg='lightgreen', fg='black', font=('Arial', 12))
    add_button.pack(side=tk.LEFT, padx=10)

    remove_button = tk.Button(button_frame, text="Remove Task", command=remove_task, bg='lightcoral', fg='black', font=('Arial', 12))
    remove_button.pack(side=tk.LEFT, padx=10)

    ChangeTask_button = tk.Button(button_frame, text="ChangeTask Status", command=ChangeTask_task_status, bg='lightblue', fg='black', font=('Arial', 12))
    ChangeTask_button.pack(side=tk.LEFT, padx=10)

    update_task_list()

    root.mainloop()

if __name__ == '__main__':
    tasks = read_tasks()
    create_gui()

