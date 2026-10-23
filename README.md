# ThinkBoard — Notes Web App - https://mern-thinkboard-hzwy.onrender.com

ThinkBoard is a full-stack notes web app. You can create, view, edit, and delete notes.

It uses **React** for the website, **Node.js and Express** for the backend, and **MongoDB** to store notes. This combination is commonly called the **MERN stack**.

## How the app works

Think of the app as a digital notebook:

- The **frontend** is the notebook you see and use in your browser.
- The **backend** is the helper that handles requests.
- The **database** is where notes are saved.
- The **API** carries messages between the frontend and backend.

```text
Browser
  React displays the website
        ↓
  Axios sends a request
        ↓
  Express routes the request
  Node.js runs the backend
        ↓
  Mongoose communicates with MongoDB
        ↓
  MongoDB stores or returns the notes
```

## Main features

- View all notes
- Create a note with a title and content
- View and edit an individual note
- Delete a note
- Display messages when actions succeed or fail
- Limit how many requests can be made in a period of time

## Technologies used

### Frontend

#### JavaScript and JSX

**JavaScript** is the programming language used by both the frontend and backend.

The React files use **JSX**, a syntax that lets you write HTML-like elements inside JavaScript.

#### React

**React** is a JavaScript library for building user interfaces. The screen is built from reusable pieces called **components**, such as a navbar, a note card, or a page.

React is technically a library rather than a complete web framework. Other tools in this project provide routing, styling, and development features.

#### React DOM

**React DOM** connects React to the browser page. It puts the React app inside the HTML element named `root`.

#### React Router

**React Router** displays different pages based on the browser address, without needing to load a completely new website each time.

The app uses these page addresses:

| Address | Page |
|---|---|
| `/` | Home page with the notes |
| `/create` | Create a new note |
| `/note/:id` | View or edit a particular note |

The `:id` part represents a note's unique ID.

#### Axios

**Axios** sends requests from the frontend to the backend. For example, the home page uses Axios to request the list of notes.

In development, the frontend sends API requests to `http://localhost:5001/api`.

#### React Hot Toast

**React Hot Toast** displays small messages, such as “Note created successfully!” or “Failed to load notes.”

#### Lucide React

**Lucide React** provides icons used in the interface, such as the plus, arrow, and trash icons.

### Styling

#### CSS

**CSS** controls how the website looks, including colors, spacing, layout, and text sizes.

#### Tailwind CSS

**Tailwind CSS** provides small styling classes that can be added directly to elements. These classes control things like padding, color, and layout.

#### DaisyUI

**DaisyUI** works with Tailwind CSS and provides ready-made styles for common interface elements, such as buttons, cards, and form fields.

#### PostCSS and Autoprefixer

**PostCSS** processes CSS using plugins. **Autoprefixer** can add browser-specific CSS prefixes so styles work across different browsers. These tools work behind the scenes during development and builds.

### Frontend development tools

#### Vite

**Vite** runs the frontend during development and builds it for deployment.

Start the development website from the `frontend` folder with:

```powershell
npm run dev
```

Vite usually serves the website at `http://localhost:5173`.

#### ESLint

**ESLint** checks JavaScript code for certain errors and code-quality problems. Run it from the `frontend` folder with:

```powershell
npm run lint
```

### Backend

#### Node.js

**Node.js** runs JavaScript outside the browser. In this project, it runs the backend server.

#### Express

**Express** is a library that runs on Node.js and makes it easier to build a web server and API. It receives requests and helps decide what the backend should do with them.

#### Routes and API

An **API** is the set of ways another program can ask the backend to do something. A **route** is one specific rule in that API.

The notes API supports these requests:

| Request | What it does |
|---|---|
| `GET /api/notes` | Get all notes |
| `GET /api/notes/:id` | Get one note by its ID |
| `POST /api/notes` | Create a note |
| `PUT /api/notes/:id` | Update a note |
| `DELETE /api/notes/:id` | Delete a note |

The route matches the request to the correct operation. A **controller** then performs that operation, such as finding or saving a note.

#### Middleware

**Middleware** is code that runs while a request travels through the backend. It can inspect or prepare the request, allow it to continue, or stop it.

This app uses middleware to:

- Read JSON sent by the frontend
- Allow the frontend and backend to communicate during development
- Apply rate limiting

#### CORS

**CORS** is a browser security mechanism that controls which websites can send requests to a server. During development, the frontend and backend use different local ports, so the backend allows the frontend's address to make requests.

#### Nodemon

**Nodemon** watches backend files during development and restarts the server when files change. Start it from the `backend` folder with:

```powershell
npm run dev
```

### Database

#### MongoDB

**MongoDB** is a database that stores information as documents. Each note is stored as a document containing information such as its title, content, and dates.

Unlike frontend state, which is temporary page memory, MongoDB keeps notes saved so they are still available after reloading the website.

#### Mongoose

**Mongoose** is a library that helps the Node.js backend communicate with MongoDB. The note model describes the fields a note should have and provides methods for saving and finding notes.

### Other backend tools

#### dotenv

**dotenv** loads settings from a `.env` file into the backend's environment. This keeps configuration—such as database addresses and service tokens—out of the main source code.

Do not share `.env` values publicly or commit them to a public repository.

#### Upstash and Redis

**Redis** is a fast data store often used for small pieces of information, such as request counters. **Upstash** provides hosted Redis, so the app can use Redis without running its own Redis server.

This project uses Upstash Redis for the rate limiter.

#### Rate limiter

A **rate limiter** restricts how many requests can be made during a period of time. This app is configured to allow up to **10 requests in a 20-second window**.

The rate limiter uses Redis to keep track of requests. In the current configuration, requests use the same counter key, so they share the limit.

## Important concepts

### Component

A **component** is a piece of the screen that can be reused, such as a navbar, note card, or page.

### State

**State** is information a component remembers while someone is using the page. For example, the create-note page keeps the title and content as you type.

State is usually temporary. To keep a note after leaving or reloading the page, it must be saved to the database.

### API

An **API** is how programs communicate. The frontend sends a request to the backend, and the backend sends a response.

### Route

A **route** is a backend rule that matches a request's method and address to an operation. For example, `POST /api/notes` is the route for creating a note.

### Controller

A **controller** contains the code that performs an operation after a route matches. It can find, create, update, or delete a note.

### Model

A **model** describes the shape of data in the database and helps the backend work with that data. A note has a title and content, along with creation and update dates.

### Middleware

**Middleware** is a checkpoint a request passes through before it reaches its route or controller. It can read, check, or prepare the request.

## What happens when a note is created?

1. The create-note page displays a form.
2. React state remembers the title and content as they are typed.
3. When the form is submitted, Axios sends a `POST /api/notes` request containing the note data.
4. Backend middleware processes the request.
5. The notes route matches the request and selects the create-note controller.
6. The controller uses the note model to save the note.
7. Mongoose communicates with MongoDB, where the note is stored.
8. The backend sends a response to the frontend.
9. The frontend shows a success message and returns to the home page.

```text
Type a note
    ↓
React component and state
    ↓
Axios API request
    ↓
Backend middleware
    ↓
Route selects the operation
    ↓
Controller performs the operation
    ↓
Mongoose communicates with MongoDB
    ↓
Response returns to the frontend
```

## Running the app locally

The frontend and backend are separate programs, so run them in two terminals.

### 1. Start the backend

In the first terminal:

```powershell
cd backend
npm run dev
```

The backend normally uses port `5001`. It also needs a valid MongoDB connection setting in `backend/.env`.

### 2. Start the frontend

In a second terminal:

```powershell
cd frontend
npm run dev
```

Open the address Vite prints in the terminal. It is usually:

```text
http://localhost:5173
```

The frontend is the website you use. Port `5001` is for backend API requests.

### Troubleshooting: port already in use

If you see `EADDRINUSE`, another program is already using the requested port. Stop the other server in its terminal with **Ctrl+C**, or identify the process using that port before stopping it.

## What does MERN mean?

MERN is a common name for these four technologies:

- **M**ongoDB — database
- **E**xpress — backend web server
- **R**eact — user interface
- **N**ode.js — runs the backend JavaScript

This project uses all four, along with supporting tools such as Vite, Axios, Mongoose, and Tailwind CSS.

## Quick summary

- **React** builds the user interface.
- **Vite** runs and builds the frontend.
- **Axios** sends requests from the frontend to the backend.
- **Node.js** runs the backend JavaScript.
- **Express** handles the backend server and API routes.
- **Middleware** checks or prepares requests.
- **Controllers** perform the requested actions.
- **Mongoose** helps the backend work with MongoDB.
- **MongoDB** stores the notes.
- **Upstash Redis** helps the rate limiter count requests.
````
