# CIT281 Lab 5

## Overview

Lab goals and outcomes:

1. Download and install Postman.
2. Create a CIT 281 collection and folders in Postman.
3. Create a Node.js and Fastify server application with GET and respond with JSON.
4. Add an array of student objects.
5. Use Postman to test server GET routes.
6. Add POST handling to the server application and respond with JSON.
7. Use Postman to test POST request.

## Deliverables

- `fastify-server.js`
- `package.json`
- `AllStudents.png`
- `SingleStudent.png`
- `Unmatched.png`
- `StudentPost.png`

These files should be placed in your `cit281/p4` folder and zipped into `lab-05.zip`.

## Instructions

### Part 1: Download and install Postman

1. Navigate to the [Postman Downloads](https://www.postman.com/downloads/) web page and download Postman.
2. Once downloaded, install Postman.

### Part 2: Create a CIT 281 collection and folders

1. Run Postman.
2. When prompted to Create an Account or Sign In, select Skip and go to the app.
3. Create a query collection called `CIT` by selecting Collections, then clicking on the plus (+).
4. Select the New Collection, then rename it to `CIT`.
5. Create a folder called `CIT 281` under the `CIT` collection.

### Part 3: Create a Node.js and Fastify server application with GET and respond with JSON

1. Create a Node.js and Fastify server file named `fastify-server.js` in your `cit281/p4` folder.
2. Copy the following code into the file:

   ```javascript
   // Require the Fastify framework and instantiate it
   const fastify = require("fastify")();

   // Array of student objects
   const students = [
     { id: 1, last: "Last1", first: "First1" },
     { id: 2, last: "Last2", first: "First2" },
     { id: 3, last: "Last3", first: "First3" },
   ];

   // GET route for all students
   fastify.get("/cit/student", (request, reply) => {
     reply
       .code(200)
       .header("Content-Type", "application/json; charset=utf-8")
       .send(students);
   });

   // GET route for a single student by ID
   fastify.get("/cit/student/:id", (request, reply) => {
     const { id } = request.params;
     const student = students.find(s => s.id === parseInt(id));
     if (student) {
       reply
         .code(200)
         .header("Content-Type", "application/json; charset=utf-8")
         .send(student);
     } else {
       reply
         .code(404)
         .header("Content-Type", "application/json; charset=utf-8")
         .send({ error: "Not Found" });
     }
   });

   // Unmatched route handler
   fastify.setNotFoundHandler((request, reply) => {
     reply
       .code(404)
       .header("Content-Type", "application/json; charset=utf-8")
       .send({ error: "Not Found" });
   });

   // Start server and listen to requests using Fastify
   const listenIP = "localhost";
   const listenPort = 8080;
   fastify.listen(listenPort, listenIP, (err, address) => {
     if (err) {
       console.log(err);
       process.exit(1);
     }
     console.log(`Server listening on ${address}`);
   });
