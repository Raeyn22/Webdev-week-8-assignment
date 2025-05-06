# Webdev-week-8-assignment

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Task Manager</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f9;
            color: #333;
            display: flex;
            flex-direction: column;
            min-height: 100vh;
        }
        header {
            background-color: #4CAF50;
            padding: 20px;
            text-align: center;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            transition: background-color 0.3s ease;
        }
        header:hover {
            background-color: #45a049;
        }
        header h1 {
            margin: 0;
            color: white;
        }
        main {
            flex: 1;
            padding: 20px;
            text-align: center;
        }
        #taskInput {
            padding: 12px;
            font-size: 16px;
            width: calc(100% - 280px);
            border: 1px solid #ddd;
            border-radius: 5px;
            margin-right: 10px;
            transition: border-color 0.3s ease;
            display: inline-block;
            box-sizing: border-box;
        }
        #taskInput:focus {
            outline: none;
            border-color: #4CAF50;
        }
        #addTaskBtn {
            padding: 12px 24px;
            font-size: 16px;
            cursor: pointer;
            border: none;
            border-radius: 5px;
            background-color: #4CAF50;
            color: white;
            transition: background-color 0.3s ease, transform 0.1s ease;
        }
        #addTaskBtn:hover {
            background-color: #45a049;
            transform: scale(1.05);
        }
        #addTaskBtn:disabled {
            background-color: #ccc;
            cursor: not-allowed;
            transform: none;
        }
        #taskList {
            margin-top: 20px;
            text-align: left;
            margin-left: auto;
            margin-right: auto;
            width: 60%;
            min-width: 300px;
        }
        #taskList ul {
            padding: 0;
            margin: 0;
            list-style: none;
        }
        #taskList li {
            background-color: white;
            padding: 15px;
            margin-bottom: 10px;
            border-radius: 5px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 1px 3px rgba(0,0,0,0.1);
            transition: background-color 0.3s ease, transform 0.1s ease;
        }
        #taskList li:hover {
            background-color: #f0f0f0;
            transform: scale(1.02);
        }
        #taskList li span {
            flex: 1;
            margin-right: 10px;
            word-break: break-word;
        }
        #taskList li.completed {
            text-decoration: line-through;
            color: gray;
        }
        #taskList button {
            padding: 8px 16px;
            font-size: 12px;
            cursor: pointer;
            border: none;
            border-radius: 5px;
            margin-left: 5px;
            transition: background-color 0.3s ease, transform 0.1s ease;
        }
        #taskList button:hover {
            transform: scale(1.05);
        }
        .completeBtn {
            background-color: #4CAF50;
            color: white;
        }
        .completeBtn:hover {
            background-color: #388E3C;
        }
        .deleteBtn {
            background-color: #f44336;
            color: white;
        }
        .deleteBtn:hover {
            background-color: #d32f2f;
        }
        footer {
            background-color: #f0f0f0;
            padding: 10px;
            text-align: center;
            margin-top: 20px;
            color: #777;
            animation: fadeIn 1s ease;
        }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        @media (max-width: 768px) {
            #taskInput {
                width: 100%;
                margin-right: 0;
                margin-bottom: 10px;
            }
            #addTaskBtn {
                width: 100%;
            }
            #taskList {
                width: 95%;
            }
        }
    </style>
</head>
<body>
    <header>
        <h1>Task Manager</h1>
    </header>
    <main>
        <div id="inputContainer">
            <input type="text" id="taskInput" placeholder="Enter task...">
            <button id="addTaskBtn">Add Task</button>
        </div>
        <div id="taskList">
            <ul>
                </ul>
        </div>
    </main>
    <footer>
        <p>&copy; 2025 Task Manager</p>
    </footer>
    <script>
        const taskInput = document.getElementById('taskInput');
        const addTaskBtn = document.getElementById('addTaskBtn');
        const taskList = document.getElementById('taskList');

        let tasks = JSON.parse(localStorage.getItem('tasks')) || [];

        function renderTasks() {
            taskList.innerHTML = '<ul></ul>';
            const taskListUL = taskList.querySelector('ul');

            tasks.forEach((task, index) => {
                const listItem = document.createElement('li');
                listItem.innerHTML = `
                    <span class="${task.completed ? 'completed' : ''}">${task.text}</span>
                    <button class="completeBtn">${task.completed ? 'Undo' : 'Complete'}</button>
                    <button class="deleteBtn">Delete</button>
                `;
                const completeBtn = listItem.querySelector('.completeBtn');
                const deleteBtn = listItem.querySelector('.deleteBtn');

                completeBtn.addEventListener('click', () => {
                    tasks[index].completed = !tasks[index].completed;
                    localStorage.setItem('tasks', JSON.stringify(tasks));
                    renderTasks();
                });

                deleteBtn.addEventListener('click', () => {
                    tasks.splice(index, 1);
                    localStorage.setItem('tasks', JSON.stringify(tasks));
                    renderTasks();
                });

                taskListUL.appendChild(listItem);
            });
            addTaskBtn.disabled = tasks.length >= 10;
        }

        addTaskBtn.addEventListener('click', () => {
            const taskText = taskInput.value.trim();
            if (taskText !== '') {
                tasks.push({ text: taskText, completed: false });
                localStorage.setItem('tasks', JSON.stringify(tasks));
                taskInput.value = '';
                renderTasks();
            }
        });

        renderTasks();
    </script>
</body>
</html>
