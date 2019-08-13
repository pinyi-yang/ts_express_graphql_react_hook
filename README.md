# Typescript Full Stack App: Node/Express, GraphQL and React (Hooks)

## Setup Node Project in Typescript
Unfortunately, Node doesn't have a --typescript flag yet like React (create-react-app my_app --typescript), that will automaticly setup a project in typescript. Following steps need to taken to setup a node project with Typscript:
1. Initiate a Node.js project with express
2. Add Typescript dependency to project

### 1. Initiate a Node.js Project
It is exactly the same as how to start a project in javascript.
**Make a app folder and get into the folder (root folder of your app)**
```
mkdir my_app
cd my_app
```

**Initiate a Node project with server.js**
In my_app/:
```
touch server.js
npm init -y
```
And add Express to the project:
```
npm i express
```
Then, to quick test the app. We can send a 'Hello World!' to root route:
```javascript
const express = require('express');
const app = express();
const PORT = 3001

app.get('/', (req, res) => {
  res.send('Hello World!');
})


app.listen(PORT, () => {
  console.log(`Port ${PORT} is connected`);
})
```
After run:
```
npm start
```
in my_app/ and go to localhost:3001, if 'Hello World!' shows in the web browser, it should be ready to go to install Typescript dependency for your App.

### 2. Setup Typescript

**Install Depedencies**

Both typescript (compiler) and ts-node need to be brought in. They can be either installed with -D (development dependency) or -g (globally):
```
npm install -g typescript ts-node 
```
If you install with -g flag, you can access typescript and ts-node without futher installation in future.

The last thing is to include type support for Express:
```
npm install @types/express
```

**Setup Typescript and Node Configurations**

With necessary dependencies ready, the next is to setup the configuration for Typescript (tsconfig.json) and Node (package.json)

*Configuration of Typescript (tsconfig.json)*

The default file for Typescript configuration is tsconfig.json. It is not included yet. We need to create it ourselves (in my_app/):
```
touch tsconfig.json
```
then add below into the tsconfig.json
```json
{
    "compilerOptions": {
        "module": "commonjs",
        "esModuleInterop": true,
        "target": "es6",
        "noImplicitAny": true,
        "moduleResolution": "node",
        "sourceMap": true,
        "outDir": "build",
        "baseUrl": ".",
        "paths": {
            "*": [
                "node_modules/*",
                "src/types/*"
            ]
        },
        "lib": [
            "es2015"
        ]
    },
    //"include": [
        //"src/**/*"
    //]
}
```
include is the source file folder, which typescript compiler will check for compiling.
outDir is the folder in the my_app/ that typescript compiler will compile .ts files to in the app root directory and direcotries in the includes. It is also related to the future setting for Node (package.json). Since in this demon our "outDir" is "build". Let's go ahead make a build folder in our my_app/

*Configuration of Node (pakcage.json)*
As the typescript will compile the server file into the my_app/build, we need to setup the test and start for the pakcage.json accordingly:
```json
{
  "name": "my_app",
  "version": "1.0.0",
  "description": "",
  "main": "build/server.js",
  "scripts": {
    "pretest": "tsc",
    "test": "nodemon build/server.js",
    "prestart": "tsc",
    "start": "node build/server.js"
  },
  "keywords": [],
  "author": "Pinyi Yang",
  "license": "ISC",
  "dependencies": {
    "@types/express": "^4.17.0",
    "express": "^4.17.1"
  }
}
```
as our main file (current: server.js, will be changed into server.ts) will be compile to my_app/build/server.js. We should set the target of "main", "test" and "start" to this file. Also notice that as we installed the @types/express for our project, we should see it here.

As we finished all the start for our Node.ts project, it is the time to change our server.js with 'Hello World!' into server.ts

*Run server test*
As soon as server.js is changed to server.ts, a message in our server.ts file will show to suggest changing require into import from, go ahead to make the change:
```typescript
// server.ts
import express from 'express';
const app = express();
const PORT = 3001

app.get('/', (req, res) => {
  res.send('Hello World!');
})


app.listen(PORT, () => {
  console.log(`Port ${PORT} is connected`);
})
```
Then in CLI, run:
```
npm start
```
As we setup the tsc (typescript compiler) in our package.json, this command will first compile our server.ts into our "outDir": my_app/build/ as server.js. Then server.js will be run.