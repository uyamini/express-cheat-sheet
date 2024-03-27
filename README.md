# express-cheat-sheet
# 1. Setup with Express Generator
## Install Express Generator Globally

npm install -g express-generator
## Scaffold a New Express App with EJS
express -e your-app-name

## Install Dependencies
cd your-app-name<br>
npm i
____________________________________________________________________________________
# 2. MVC Code Organization


## Create directories for models, views, and controllers if they don't already exist
mkdir models controllers

### Example Model (models/todo.js)
const todos = [<br>
  { id: 1, todo: "Learn Express", done: false },<br>
  // more todos<br>
];<br>

module.exports = {<br>
  getAll() {<br>
    return todos;<br>
  },<br>
  getOne(id) {<br>
    return todos.find(todo => todo.id === parseInt(id));<br>
  },<br>
  // additional model functions<br>
};<br>


### Example View (views/todos/index.ejs)

\<ul><br>
  <% todos.forEach(function(todo) { %><br>
    \<li><%= todo.todo %> - <%= todo.done ? 'Done' : 'Pending' %>\</li><br>
  <% }); %><br>
\</ul><br>

    
### Example Controller (controllers/todos.js)

const Todo = require('../models/todo');<br>

module.exports = {<br>
  index(req, res) {<br>
    res.render('todos/index', { todos: Todo.getAll() });<br>
  },<br>
  show(req, res) {<br>
    res.render('todos/show', { todo: Todo.getOne(req.params.id) });<br>
  },<br>
  // additional controller actions<br>
};<br>

_______________________________________________________________________
# 3. Best Practice Routing
### Example Routing (routes/todos.js)

const express = require('express');<br>
const router = express.Router();<br>
const todosCtrl = require('../controllers/todos');<br>

router.get('/', todosCtrl.index); // List all todos<br>
router.get('/:id', todosCtrl.show); // Show a single todo<br>

module.exports = router;<br>

### Mount Router in server.js
const todosRouter = require('./routes/todos');<br>

app.use('/todos', todosRouter);<br>

________________________________________________________________________
# 4. Implementing Functionality
   
### Example - Show Function in Controller (controllers/todos.js)
show(req, res) {<br>
  const todo = Todo.getOne(req.params.id);<br>
  if (todo) {<br>
    res.render('todos/show', { todo });<br>
  } else {<br>
    res.status(404).send('Todo not found');<br>
  }<br>
}<br>

### Route for Showing a Single Todo (routes/todos.js)
router.get('/:id', todosCtrl.show);<br>

### Linking to the Show Page (views/todos/index.ejs)

\<ul><br>
  <% todos.forEach(function(todo) { %><br>
    \<li>\<a href="/todos/<%= todo.id %>"><%= todo.todo %></a></li><br>
  <% }); %><br>
\</ul><br>
    
### Show View (views/todos/show.ejs)

\<h2><%= todo.todo %>\</h2><br>
\<p>Status: <%= todo.done ? 'Done' : 'Pending' %>\</p><br>
