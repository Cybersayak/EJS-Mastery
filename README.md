# Embedded JS Mastery

###  EJS is a simple templating language that lets you generate HTML markup with plain JavaScript. 


---

### **What is EJS?**
EJS (Embedded JavaScript Templating) is a simple templating language that lets you generate HTML with plain JavaScript. It's commonly used in Node.js applications to dynamically render HTML pages by embedding JavaScript logic directly into your templates.

Key Features:
1. Lightweight and easy to learn.
2. Allows embedding JavaScript code inside HTML.
3. Supports reusable components (partials).
4. Integrates seamlessly with Express.js.

---

### **Step 1: Setting Up Your Environment**

Before diving into EJS, ensure you have the following installed:
1. **Node.js**: Download and install it from [nodejs.org](https://nodejs.org/).
2. **npm (Node Package Manager)**: Comes bundled with Node.js.

#### Steps:
1. Create a project folder:
   ```bash
   mkdir ejs-tutorial
   cd ejs-tutorial
   ```

2. Initialize a new Node.js project:
   ```bash
   npm init -y
   ```

3. Install EJS:
   ```bash
   npm install ejs
   ```

4. Install Express.js (optional but recommended for web apps):
   ```bash
   npm install express
   ```

---

### **Step 2: Basic Syntax of EJS**

EJS uses special tags to embed JavaScript into HTML. Here are the most common tags:

1. `<% %>`: Executes JavaScript code (no output).
2. `<%= %>`: Outputs the result of an expression (HTML-escaped).
3. `<%- %>`: Outputs raw HTML (not escaped).
4. `<%# %>`: Adds comments (ignored during rendering).

#### Example:
```html
<!DOCTYPE html>
<html>
<head>
  <title>EJS Example</title>
</head>
<body>
  <h1>Hello, <%= name %>!</h1>
  <% if (isAdmin) { %>
    <p>Welcome, Admin!</p>
  <% } else { %>
    <p>Welcome, User!</p>
  <% } %>
</body>
</html>
```

In this example:
- `<%= name %>` outputs the value of `name`.
- `<% if (isAdmin) { %>` executes conditional logic.

---

### **Step 3: Rendering an EJS Template**

Let’s create a simple EJS file and render it using Node.js.

#### 1. Create an EJS File:
Create a file named `index.ejs` in a folder called `views`:
```html
<!-- views/index.ejs -->
<!DOCTYPE html>
<html>
<head>
  <title>EJS Tutorial</title>
</head>
<body>
  <h1>Welcome to EJS!</h1>
  <p>Your name is: <%= name %></p>
</body>
</html>
```

#### 2. Render the Template Using Node.js:
Create a file named `app.js` in the root directory:
```javascript
const ejs = require('ejs');
const fs = require('fs');

// Read the EJS template
const template = fs.readFileSync('./views/index.ejs', 'utf-8');

// Data to pass to the template
const data = { name: 'John Doe' };

// Render the template
const renderedHtml = ejs.render(template, data);

console.log(renderedHtml);
```

Run the script:
```bash
node app.js
```

Output:
```html
<!DOCTYPE html>
<html>
<head>
  <title>EJS Tutorial</title>
</head>
<body>
  <h1>Welcome to EJS!</h1>
  <p>Your name is: John Doe</p>
</body>
</html>
```

---

### **Step 4: Using EJS with Express.js**

Express.js simplifies the process of rendering EJS templates in a web application.

#### 1. Set Up Express:
Update `app.js` to use Express:
```javascript
const express = require('express');
const app = express();

// Set EJS as the templating engine
app.set('view engine', 'ejs');

// Define a route
app.get('/', (req, res) => {
  const data = { name: 'Alice' };
  res.render('index', data); // Renders views/index.ejs
});

// Start the server
app.listen(3000, () => {
  console.log('Server is running on http://localhost:3000');
});
```

#### 2. Folder Structure:
Ensure your folder structure looks like this:
```
ejs-tutorial/
├── views/
│   └── index.ejs
├── app.js
├── package.json
└── node_modules/
```

#### 3. Run the Server:
```bash
node app.js
```

Visit `http://localhost:3000` in your browser. You should see:
```
Welcome to EJS!
Your name is: Alice
```

---

### **Step 5: Loops and Conditionals**

EJS allows you to use loops and conditionals to dynamically generate content.

#### Example:
```html
<!-- views/loop.ejs -->
<ul>
  <% users.forEach(user => { %>
    <li><%= user.name %> - <%= user.age %> years old</li>
  <% }); %>
</ul>
```

Update `app.js`:
```javascript
app.get('/users', (req, res) => {
  const users = [
    { name: 'Alice', age: 25 },
    { name: 'Bob', age: 30 },
    { name: 'Charlie', age: 35 }
  ];
  res.render('loop', { users });
});
```

Visit `http://localhost:3000/users`. You’ll see:
```
• Alice - 25 years old
• Bob - 30 years old
• Charlie - 35 years old
```

---

### **Step 6: Partials (Reusing Components)**

Partials allow you to reuse pieces of HTML across multiple templates.

#### Example:
1. Create a partial file `views/partials/header.ejs`:
```html
<header>
  <h1>My Website</h1>
</header>
```

2. Include the partial in `views/index.ejs`:
```html
<%- include('partials/header') %>
<p>Welcome to the homepage!</p>
```

Now, the header will be included in every page where you use `<%- include('partials/header') %>`.

---

### **Step 7: Advanced Topics**

#### 1. **Custom Delimiters**:
You can change the default delimiters (`<% %>`) to something else:
```javascript
app.set('view engine', 'ejs');
app.locals.settings['view options'] = { delimiter: '?' };
```

Use `<? ?>` instead of `<% %>` in your templates.

#### 2. **Escaping vs. Raw Output**:
- `<%= %>` escapes HTML to prevent XSS attacks.
- `<%- %>` outputs raw HTML (useful for including partials).

#### 3. **Caching Templates**:
In production, enable caching for better performance:
```javascript
app.set('view cache', true);
```

---

### **Step 8: Best Practices**

1. **Organize Your Files**:
   - Use a `views` folder for templates.
   - Use a `partials` folder for reusable components.

2. **Keep Logic Minimal**:
   Avoid complex logic in templates. Move heavy computations to your backend code.

3. **Use Helpers**:
   Create helper functions for repetitive tasks (e.g., formatting dates).

---

