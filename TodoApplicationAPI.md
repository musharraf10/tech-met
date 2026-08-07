# Todo Application API

An Express.js REST API with an integrated SQLite database for managing a Todo list with filtering, updates, insertion, and deletion capabilities.

---

## 📌 Features

* **Query-based Filtering:** Filter todos by priority, status, or search query string.
* **RESTful Endpoints:** Complete CRUD operations (Create, Read, Update, Delete).
* **SQLite Database:** Automatic database initialization and schema creation on server startup.

---

## 🛠 Project Setup & Installation

### 1. Prerequisites

Ensure you have **Node.js** and **npm** installed on your system.

### 2. Install Dependencies

Initialize your project and install the required packages:

```bash
npm init -y
npm install express sqlite sqlite3

```

---

## 🗄 Database Schema

The application automatically creates a `todo` table in `todoApplication.db` with the following structure:

| Field | Type | Constraint |
| --- | --- | --- |
| `id` | `INTEGER` | `PRIMARY KEY` |
| `todo` | `TEXT` | — |
| `priority` | `TEXT` | — |
| `status` | `TEXT` | — |

---

## 🚀 API Endpoints Overview

| Method | Endpoint | Description | Query / Body Parameters |
| --- | --- | --- | --- |
| `GET` | `/todos/` | Get list of todos with optional filters | Query: `search_q`, `priority`, `status` |
| `GET` | `/todos/:todoId/` | Get a specific todo by ID | Path Param: `todoId` |
| `POST` | `/todos/` | Create a new todo item | Body: `{ id, todo, priority, status }` |
| `PUT` | `/todos/:todoId/` | Update details of an existing todo | Body: `{ todo }`, `{ priority }`, or `{ status }` |
| `DELETE` | `/todos/:todoId/` | Delete a todo item by ID | Path Param: `todoId` |

---

## 💻 Source Code (`app.js`)

Below is the complete implementation of the application:

```javascript
const express = require('express')
const path = require('path')
const {open} = require('sqlite')
const sqlite3 = require('sqlite3')

const app = express()
app.use(express.json())

const dbPath = path.join(__dirname, 'todoApplication.db')
let db = null

const initializeDBAndServer = async () => {
  try {
    db = await open({
      filename: dbPath,
      driver: sqlite3.Database,
    })

    // Create 'todo' table if it doesn't exist
    await db.exec(`
      CREATE TABLE IF NOT EXISTS todo (
        id INTEGER PRIMARY KEY,
        todo TEXT,
        priority TEXT,
        status TEXT
      );
    `)

    app.listen(3000, () => {
      console.log('Server Running at http://localhost:3000/')
    })
  } catch (e) {
    console.log(`DB Error: ${e.message}`)
    process.exit(1)
  }
}

initializeDBAndServer()

// Helper functions for Query Params evaluation
const hasPriorityAndStatusProperties = requestQuery => {
  return (
    requestQuery.priority !== undefined && requestQuery.status !== undefined
  )
}

const hasPriorityProperty = requestQuery => {
  return requestQuery.priority !== undefined
}

const hasStatusProperty = requestQuery => {
  return requestQuery.status !== undefined
}

// API 1: Get todos based on status, priority, or search query
app.get('/todos/', async (request, response) => {
  let getTodosQuery = ''
  const {search_q = '', priority, status} = request.query

  switch (true) {
    case hasPriorityAndStatusProperties(request.query):
      getTodosQuery = `
        SELECT
          *
        FROM
          todo
        WHERE
          todo LIKE '%${search_q}%'
          AND status = '${status}'
          AND priority = '${priority}';`
      break

    case hasPriorityProperty(request.query):
      getTodosQuery = `
        SELECT
          *
        FROM
          todo
        WHERE
          todo LIKE '%${search_q}%'
          AND priority = '${priority}';`
      break

    case hasStatusProperty(request.query):
      getTodosQuery = `
        SELECT
          *
        FROM
          todo
        WHERE
          todo LIKE '%${search_q}%'
          AND status = '${status}';`
      break

    default:
      getTodosQuery = `
        SELECT
          *
        FROM
          todo
        WHERE
          todo LIKE '%${search_q}%';`
  }

  const data = await db.all(getTodosQuery)
  response.send(data)
})

// API 2: Get specific todo by ID
app.get('/todos/:todoId/', async (request, response) => {
  const {todoId} = request.params
  const getTodoQuery = `
    SELECT
      *
    FROM
      todo
    WHERE
      id = ${todoId};`

  const todo = await db.get(getTodoQuery)
  response.send(todo)
})

// API 3: Add new todo
app.post('/todos/', async (request, response) => {
  const {id, todo, priority, status} = request.body
  const addTodoQuery = `
    INSERT INTO
      todo (id, todo, priority, status)
    VALUES
      (${id}, '${todo}', '${priority}', '${status}');`

  await db.run(addTodoQuery)
  response.send('Todo Successfully Added')
})

// API 4: Update todo details by ID
app.put('/todos/:todoId/', async (request, response) => {
  const {todoId} = request.params
  let updateColumn = ''
  const requestBody = request.body

  switch (true) {
    case requestBody.status !== undefined:
      updateColumn = 'Status'
      break
    case requestBody.priority !== undefined:
      updateColumn = 'Priority'
      break
    case requestBody.todo !== undefined:
      updateColumn = 'Todo'
      break
  }

  const previousTodoQuery = `
    SELECT
      *
    FROM
      todo
    WHERE
      id = ${todoId};`
  const previousTodo = await db.get(previousTodoQuery)

  const {
    todo = previousTodo.todo,
    priority = previousTodo.priority,
    status = previousTodo.status,
  } = request.body

  const updateTodoQuery = `
    UPDATE
      todo
    SET
      todo='${todo}',
      priority='${priority}',
      status='${status}'
    WHERE
      id = ${todoId};`

  await db.run(updateTodoQuery)
  response.send(`${updateColumn} Updated`)
})

// API 5: Delete a todo by ID
app.delete('/todos/:todoId/', async (request, response) => {
  const {todoId} = request.params
  const deleteTodoQuery = `
    DELETE FROM
      todo
    WHERE
      id = ${todoId};`

  await db.run(deleteTodoQuery)
  response.send('Todo Deleted')
})

module.exports = app

```

---

## 🏃 Execution

Start the application with Node:

```bash
node app.js

```

The server will start at `http://localhost:3000/`.
