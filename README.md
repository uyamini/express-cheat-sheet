# express-cheat-sheet
# 1. Setup with Express Generator
Install Express Generator Globally

# npm install -g express-generator
Scaffold a New Express App with EJS
express -e <your-app-name>

# Install Dependencies
cd <your-app-name>
npm i
____________________________________________________________________________________
# 2. MVC Code Organization


# Create directories for models, views, and controllers if they don't already exist
mkdir models controllers

# Example Model (models/todo.js)
const todos = [
  { id: 1, todo: "Learn Express", done: false },
  // more todos
];

module.exports = {
  getAll() {
    return todos;
  },
  getOne(id) {
    return todos.find(todo => todo.id === parseInt(id));
  },
  // additional model functions
};


# Example View (views/todos/index.ejs)

<ul>
  <% todos.forEach(function(todo) { %>
    <li><%= todo.todo %> - <%= todo.done ? 'Done' : 'Pending' %></li>
  <% }); %>
</ul>

    
# Example Controller (controllers/todos.js)

const Todo = require('../models/todo');

module.exports = {
  index(req, res) {
    res.render('todos/index', { todos: Todo.getAll() });
  },
  show(req, res) {
    res.render('todos/show', { todo: Todo.getOne(req.params.id) });
  },
  // additional controller actions
};

_______________________________________________________________________
# 3. Best Practice Routing
Example Routing (routes/todos.js)

const express = require('express');
const router = express.Router();
const todosCtrl = require('../controllers/todos');

router.get('/', todosCtrl.index); // List all todos
router.get('/:id', todosCtrl.show); // Show a single todo

module.exports = router;

# Mount Router in server.js
const todosRouter = require('./routes/todos');

app.use('/todos', todosRouter);

________________________________________________________________________
# 4. Implementing Functionality
   
# Example - Show Function in Controller (controllers/todos.js)
show(req, res) {
  const todo = Todo.getOne(req.params.id);
  if (todo) {
    res.render('todos/show', { todo });
  } else {
    res.status(404).send('Todo not found');
  }
}

# Route for Showing a Single Todo (routes/todos.js)
router.get('/:id', todosCtrl.show);

# Linking to the Show Page (views/todos/index.ejs)

<ul>
  <% todos.forEach(function(todo) { %>
    <li><a href="/todos/<%= todo.id %>"><%= todo.todo %></a></li>
  <% }); %>
</ul>
    
# Show View (views/todos/show.ejs)
<h2><%= todo.todo %></h2>
<p>Status: <%= todo.done ? 'Done' : 'Pending' %></p>
