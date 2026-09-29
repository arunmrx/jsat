<!DOCTYPE html>
<html>
<head>
    <title>To-Do List</title>
</head>

<body>

    <h2>My To-Do List</h2>

    <input type="text" id="taskInput" placeholder="Enter a task">

    <button onclick="addTask()">Add Task</button>
    <button onclick="displayTasks()">Display Tasks</button>

    <ul id="taskList"></ul>

    <script>

        let tasks = [];

        function addTask() {

            let task = document.getElementById("taskInput").value;

            if (task === "") {
                alert("Please enter a task");
                return;
            }

            tasks.push(task);

            document.getElementById("taskInput").value = "";

            alert("Task added successfully!");
        }

        function displayTasks() {

            let taskList = document.getElementById("taskList");

            taskList.innerHTML = "";

            tasks.forEach(function(task, index) {

                let li = document.createElement("li");

                let taskText = document.createElement("span");
                taskText.textContent = task;

                let completeButton = document.createElement("button");
                completeButton.textContent = "Mark Completed";

                completeButton.onclick = function() {
                    taskText.textContent = "✓ " + task;
                };

                let deleteButton = document.createElement("button");
                deleteButton.textContent = "Delete";

                deleteButton.onclick = function() {
                    tasks.splice(index, 1);
                    displayTasks();
                };

                li.appendChild(taskText);
                li.appendChild(document.createTextNode(" "));

                li.appendChild(completeButton);
                li.appendChild(document.createTextNode(" "));

                li.appendChild(deleteButton);

                taskList.appendChild(li);
            });
        }

    </script>

</body>
</html>.      




  <!DOCTYPE html>
<html>

<head>
    <title>Interactive Calculator</title>
</head>

<body>

    <!-- Question:
         Create an Interactive Calculator integrating variables,
         functions and DOM Manipulation using JavaScript.
    -->

    <h2>Interactive Calculator</h2>

    <div id="calc">
        <input type="text" id="display" disabled>
        <br><br>
    </div>

    <script>

        let expression = "";

        const display = document.getElementById("display");
        const calcDiv = document.getElementById("calc");

        const buttons = [
            "7", "8", "9", "/",
            "4", "5", "6", "*",
            "1", "2", "3", "-",
            "0", ".", "=", "+",
            "C"
        ];

        buttons.forEach(function(label) {

            const btn = document.createElement("button");

            btn.textContent = label;

            btn.addEventListener("click", function() {
                handleClick(label);
            });

            calcDiv.appendChild(btn);
        });


        function handleClick(label) {

            if (label === "C") {

                expression = "";

            }

            else if (label === "=") {

                try {
                    expression = Function(
                        "return " + expression
                    )().toString();
                }

                catch (e) {
                    expression = "Error";
                }

            }

            else {

                expression += label;

            }

            display.value = expression;
        }

    </script>

</body>

</html>


/*
Question:
To build a web server using Express.js with multiple routes
handling different HTTP requests.
*/

const express = require("express");

const app = express();

const PORT = 3000;


// Home Route
app.get("/", (req, res) => {
    res.send(`
        <h1>Home Page</h1>
        <p>Welcome to my website.</p>
    `);
});


// About Route
app.get("/about", (req, res) => {
    res.send(`
        <h1>About Page</h1>
        <p>This is the About page.</p>
    `);
});


// Services Route
app.get("/services", (req, res) => {
    res.send(`
        <h1>Services Page</h1>
        <p>This is the list of our services.</p>
    `);
});


// Contact Route
app.get("/contact", (req, res) => {
    res.send(`
        <h1>Contact Page</h1>
        <p>Contact us at info@example.com</p>
    `);
});


// Start Server
app.listen(PORT, () => {
    console.log(`Server running on http://localhost:${PORT}`);
});







/*
Question:
To write custom Express middleware for request logging
and centralized error handling, and use dynamic route parameters.
*/

const express = require("express");
const fs = require("fs");

const app = express();


// Custom Logging Middleware
app.use((req, res, next) => {

    const log = `${new Date().toISOString()} - ${req.method} - ${req.url}\n`;

    console.log(log.trim());

    fs.appendFile("log.txt", log, (err) => {
        if (err) {
            console.log("Error writing log file");
        }
    });

    next();
});


// Dynamic User Route
app.get("/users/:id", (req, res) => {

    res.send(`User details for ID: ${req.params.id}`);

});


// Dynamic Product Route
app.get("/product/:category/:id", (req, res) => {

    res.send(
        `Category: ${req.params.category}, Product ID: ${req.params.id}`
    );

});


// Route to generate an error
app.get("/error", (req, res) => {

    throw new Error("Something went wrong!");

});


// Centralized Error Handling Middleware
app.use((err, req, res, next) => {

    console.error(err.stack);

    res.status(500).send("Something broke on the server!");

});


// Start Server
app.listen(3000, () => {

    console.log("Server running on port 3000");

});
