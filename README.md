# task-2
to do list
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>To-Do List</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      background-color: #f4f4f4;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }

    #todo-container {
      width: 300px;
      background-color: #fff;
      border-radius: 8px;
      padding: 20px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }

    #task-list {
      list-style: none;
      padding: 0;
    }

    .task {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 10px;
      border-bottom: 1px solid #ccc;
    }

    .task input[type="checkbox"] {
      margin-right: 10px;
    }

    #new-task {
      margin-top: 10px;
    }
  </style>
</head>
<body>

<div id="todo-container">
  <h2>To-Do List</h2>
  <ul id="task-list"></ul>
  <div>
    <input type="text" id="new-task" placeholder="New task...">
    <button onclick="addTask()">Add Task</button>
  </div>
</div>

<script>
  document.addEventListener("DOMContentLoaded", function () {
    // Get task list from local storage
    const tasks = JSON.parse(localStorage.getItem("tasks")) || [];

    // Render tasks
    renderTasks();

    // Add event listener for "Enter" key to add a new task
    document.getElementById("new-task").addEventListener("keyup", function (event) {
      if (event.key === "Enter") {
        addTask();
      }
    });

    // Function to add a new task
    window.addTask = function () {
      const newTaskInput = document.getElementById("new-task");
      const taskText = newTaskInput.value.trim();

      if (taskText === "") {
        alert("Please enter a task.");
        return;
      }

      // Add the new task to the tasks array
      tasks.push({ text: taskText, completed: false });

      // Save tasks to local storage
      localStorage.setItem("tasks", JSON.stringify(tasks));

      // Render tasks
      renderTasks();

      // Clear the input field
      newTaskInput.value = "";
    };

    // Function to render tasks
    function renderTasks() {
      const taskList = document.getElementById("task-list");
      taskList.innerHTML = "";

      tasks.forEach(function (task, index) {
        const listItem = document.createElement("li");
        listItem.classList.add("task");

        const checkbox = document.createElement("input");
        checkbox.type = "checkbox";
        checkbox.checked = task.completed;
        checkbox.addEventListener("change", function () {
          toggleTask(index);
        });

        const taskText = document.createElement("span");
        taskText.innerText = task.text;

        const deleteButton = document.createElement("button");
        deleteButton.innerText = "Delete";
        deleteButton.addEventListener("click", function () {
          deleteTask(index);
        });

        listItem.appendChild(checkbox);
        listItem.appendChild(taskText);
        listItem.appendChild(deleteButton);

        taskList.appendChild(listItem);
      });
    }

    // Function to toggle task completion status
    function toggleTask(index) {
      tasks[index].completed = !tasks[index].completed;

      // Save tasks to local storage
      localStorage.setItem("tasks", JSON.stringify(tasks));

      // Render tasks
      renderTasks();
    }

    // Function to delete a task
    function deleteTask(index) {
      tasks.splice(index, 1);

      // Save tasks to local storage
      localStorage.setItem("tasks", JSON.stringify(tasks));

      // Render tasks
      renderTasks();
    }
  });
</script>

</body>
</html>
