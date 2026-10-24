\# CodeCraftHub — Personal Learning Goal Tracker API



\## Project Overview



CodeCraftHub is a simple REST API that helps developers track the courses they want to learn. You can \*\*create, read, update, and delete\*\* courses, and see statistics about your progress. Data is stored in a lightweight JSON file (`courses.json`), so no database and no authentication are needed.



The server is built with \*\*Node.js + Express\*\* and exposes its endpoints under `/api/courses`.



\---



\## Features



\- CRUD operations for courses (Create, Read, Update, Delete)

\- Statistics endpoint: total courses and count by status

\- Data stored in `courses.json`, created automatically on first use

\- Course fields:



| Field         | Type   | Notes                                                        |

|---------------|--------|--------------------------------------------------------------|

| `id`          | number | Auto-generated, starts from 1                                |

| `name`        | string | Required                                                     |

| `description` | string | Required                                                     |

| `target\_date` | string | Required, format `YYYY-MM-DD`                                |

| `status`      | string | Required: `"Not Started"`, `"In Progress"`, or `"Completed"` |

| `created\_at`  | string | Auto-generated timestamp (UTC), e.g. `2026-10-24 20:01:52`   |



\- Error handling for:

&#x20; - Missing required fields (400)

&#x20; - Invalid status values (400)

&#x20; - Course not found (404)

&#x20; - File read/write errors (500)



\---



\## Project Structure



```

codecrafthub/

├── app.js            # Express application (all routes and helpers)

├── courses.json      # Data storage (auto-created)

├── package.json      # Project info and dependencies

└── README.md         # Project documentation

```



\---



\## Installation



1\. Install \*\*Node.js\*\* (LTS version) from https://nodejs.org

2\. Clone the repository and move into the folder:



```bash

git clone https://github.com/jpark143/codecrafthub.git

cd codecrafthub

```



3\. Install dependencies:



```bash

npm install

```



\---



\## How to Run the Application



```bash

npm start

```



or



```bash

node app.js

```



You should see:



```

============================================================

CodeCraftHub API is starting...

============================================================

Data will be stored in: <path>\\codecrafthub\\courses.json

API is available at: http://localhost:5000

============================================================

```



Keep this terminal open while you use the API. Press \*\*Ctrl+C\*\* to stop the server.



\---



\## API Endpoint Documentation



Base URL: `http://localhost:5000/api/courses`



| Method | Endpoint             | Description              |

|--------|----------------------|--------------------------|

| POST   | `/api/courses`       | Add a new course         |

| GET    | `/api/courses`       | Get all courses          |

| GET    | `/api/courses/stats` | Get course statistics    |

| GET    | `/api/courses/:id`   | Get a specific course    |

| PUT    | `/api/courses/:id`   | Update a course          |

| DELETE | `/api/courses/:id`   | Delete a course          |



> The curl examples below use macOS/Linux syntax. For Windows Command Prompt, see \[Troubleshooting](#troubleshooting).



\### 1) Add a new course



`POST /api/courses`



```bash

curl -X POST http://localhost:5000/api/courses \\

&#x20; -H "Content-Type: application/json" \\

&#x20; -d '{

&#x20;   "name": "Python Basics",

&#x20;   "description": "Learn Python fundamentals",

&#x20;   "target\_date": "2025-12-31",

&#x20;   "status": "Not Started"

&#x20; }'

```



Response `201 Created`:



```json

{

&#x20; "success": true,

&#x20; "message": "Course added successfully",

&#x20; "course": {

&#x20;   "id": 1,

&#x20;   "name": "Python Basics",

&#x20;   "description": "Learn Python fundamentals",

&#x20;   "target\_date": "2025-12-31",

&#x20;   "status": "Not Started",

&#x20;   "created\_at": "2026-10-24 20:01:52"

&#x20; }

}

```



\### 2) Get all courses



`GET /api/courses`



```bash

curl http://localhost:5000/api/courses

```



Response `200 OK`:



```json

{

&#x20; "success": true,

&#x20; "count": 1,

&#x20; "courses": \[

&#x20;   {

&#x20;     "id": 1,

&#x20;     "name": "Python Basics",

&#x20;     "description": "Learn Python fundamentals",

&#x20;     "target\_date": "2025-12-31",

&#x20;     "status": "Not Started",

&#x20;     "created\_at": "2026-10-24 20:01:52"

&#x20;   }

&#x20; ]

}

```



\### 3) Get course statistics



`GET /api/courses/stats`



```bash

curl http://localhost:5000/api/courses/stats

```



Response `200 OK`:



```json

{

&#x20; "success": true,

&#x20; "stats": {

&#x20;   "total\_courses": 1,

&#x20;   "by\_status": {

&#x20;     "Not Started": 0,

&#x20;     "In Progress": 1,

&#x20;     "Completed": 0

&#x20;   }

&#x20; }

}

```



\### 4) Get a specific course



`GET /api/courses/:id`



```bash

curl http://localhost:5000/api/courses/1

```



Response `200 OK`:



```json

{

&#x20; "success": true,

&#x20; "course": {

&#x20;   "id": 1,

&#x20;   "name": "Python Basics",

&#x20;   "description": "Learn Python fundamentals",

&#x20;   "target\_date": "2025-12-31",

&#x20;   "status": "Not Started",

&#x20;   "created\_at": "2026-10-24 20:01:52"

&#x20; }

}

```



\### 5) Update a course



`PUT /api/courses/:id`



Send only the fields you want to change. Other fields keep their current values.



```bash

curl -X PUT http://localhost:5000/api/courses/1 \\

&#x20; -H "Content-Type: application/json" \\

&#x20; -d '{ "status": "In Progress" }'

```



Response `200 OK`:



```json

{

&#x20; "success": true,

&#x20; "message": "Course updated successfully",

&#x20; "course": {

&#x20;   "id": 1,

&#x20;   "name": "Python Basics",

&#x20;   "description": "Learn Python fundamentals",

&#x20;   "target\_date": "2025-12-31",

&#x20;   "status": "In Progress",

&#x20;   "created\_at": "2026-10-24 20:01:52"

&#x20; }

}

```



\### 6) Delete a course



`DELETE /api/courses/:id`



```bash

curl -X DELETE http://localhost:5000/api/courses/1

```



Response `200 OK`:



```json

{

&#x20; "success": true,

&#x20; "message": "Course deleted successfully",

&#x20; "deleted\_course": {

&#x20;   "id": 1,

&#x20;   "name": "Python Basics",

&#x20;   "description": "Learn Python fundamentals",

&#x20;   "target\_date": "2025-12-31",

&#x20;   "status": "In Progress",

&#x20;   "created\_at": "2026-10-24 20:01:52"

&#x20; }

}

```



\### Error responses



| Status | When                                      | Example response                                                                              |

|--------|-------------------------------------------|-----------------------------------------------------------------------------------------------|

| 400    | Empty request body                        | `{"success":false,"error":"No data provided"}`                                                |

| 400    | Missing required fields                   | `{"success":false,"error":"Missing required fields: description, target\_date, status"}`       |

| 400    | Invalid status value                      | `{"success":false,"error":"Status must be one of: Not Started, In Progress, Completed"}`      |

| 404    | Course ID does not exist                  | `{"success":false,"error":"Course with ID 99 not found"}`                                     |

| 500    | File could not be saved                   | `{"success":false,"error":"Failed to save course"}`                                           |



\---



\## Troubleshooting



\*\*`npm ERR! enoent ... package.json`\*\*

You are not in the project folder. Run `cd codecrafthub` (Windows: `cd %USERPROFILE%\\Documents\\codecrafthub`) and try again.



\*\*`curl: (7) Failed to connect to localhost:5000`\*\*

The server is not running. Start it with `npm start` in one terminal and run curl commands in a second terminal.



\*\*`Error: listen EADDRINUSE :::5000`\*\*

Port 5000 is already in use. Stop the other process, or change `PORT` in `app.js`. On macOS, the AirPlay Receiver often uses port 5000; turn it off in System Settings → General → AirDrop \& Handoff.



\*\*`Cannot find module 'express'`\*\*

Run `npm install` in the project folder.



\*\*curl POST/PUT fails on Windows Command Prompt\*\*

Windows cmd does not support single quotes or `\\` line breaks. Write the command on one line and escape inner double quotes:



```

curl -X POST http://localhost:5000/api/courses -H "Content-Type: application/json" -d "{\\"name\\":\\"Python Basics\\",\\"description\\":\\"Learn Python fundamentals\\",\\"target\_date\\":\\"2025-12-31\\",\\"status\\":\\"Not Started\\"}"

```



In PowerShell, use `curl.exe` instead of `curl`.



\*\*`Status must be one of: ...`\*\*

Use the exact values, including capitalization and spaces: `Not Started`, `In Progress`, `Completed`.



\*\*`/api/courses/stats` returns "Course with ID NaN not found"\*\*

The stats route must be defined in `app.js` \*\*before\*\* the `/api/courses/:id` route, otherwise Express treats `stats` as an ID.



\*\*Courses disappeared or data looks wrong\*\*

Make sure `courses.json` contains valid JSON (an array such as `\[]`). If it is broken, delete it and restart the server; it will be recreated automatically.



\*\*`created\_at` time looks a few hours off\*\*

Timestamps are stored in UTC, not your local time zone.



\---



\## Possible Improvements



\- Validate the `target\_date` format (`YYYY-MM-DD`)

\- Add automated tests with Jest and Supertest

\- Add search and sort-by-date endpoints

\- Move to a database such as MongoDB as the project grows





