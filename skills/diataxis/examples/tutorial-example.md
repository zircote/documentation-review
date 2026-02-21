---
diataxis_type: tutorial
diataxis_learning_goals:
  - Set up a local development environment
  - Create and run a basic application
  - Understand the project structure
---

# Tutorial: Build Your First Task Manager

## What you'll learn

In this tutorial, we will build a simple task manager from scratch. Along the way, you will learn how to set up the development environment, create the core data model, and run the application locally.

## Prerequisites

- Node.js 20 or later installed on your machine
- A text editor

## Steps

### Step 1: Create the project

Let's start by creating a new project directory and initializing it.

```bash
mkdir my-task-manager
cd my-task-manager
npm init -y
```

You should see output confirming the `package.json` was created:

```
Wrote to /path/to/my-task-manager/package.json
```

### Step 2: Install dependencies

Now, install the core dependency we need:

```bash
npm install express
```

You will notice a `node_modules/` directory has appeared and your `package.json` now lists `express` under dependencies.

### Step 3: Create the application

Create a file called `index.js`:

```javascript
const express = require('express');
const app = express();

app.use(express.json());

const tasks = [];

app.get('/tasks', (req, res) => {
  res.json(tasks);
});

app.post('/tasks', (req, res) => {
  const task = { id: tasks.length + 1, title: req.body.title, done: false };
  tasks.push(task);
  res.status(201).json(task);
});

app.listen(3000, () => {
  console.log('Task manager running on http://localhost:3000');
});
```

### Step 4: Run the application

Start the server:

```bash
node index.js
```

The output should look like:

```
Task manager running on http://localhost:3000
```

Let's verify it works by creating a task. Open a new terminal and run:

```bash
curl -X POST http://localhost:3000/tasks \
  -H "Content-Type: application/json" \
  -d '{"title": "Learn Diátaxis"}'
```

You should see a response like:

```json
{"id": 1, "title": "Learn Diátaxis", "done": false}
```

### Step 5: View your tasks

Now let's check that our task was saved:

```bash
curl http://localhost:3000/tasks
```

The output should show the task we just created:

```json
[{"id": 1, "title": "Learn Diátaxis", "done": false}]
```

## What you've accomplished

You have built a working task manager with a REST API that can create and list tasks. Along the way, you learned how to initialize a Node.js project, set up an Express server, and handle JSON requests.

## Next steps

- [How to add authentication to your API](../guides/add-authentication.md)
- [How to deploy to production](../guides/deploy-to-production.md)
- [Understanding REST API design](../explanation/rest-api-design.md)
- [Express API reference](../reference/express-api.md)
