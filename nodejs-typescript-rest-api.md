# NODEJS TYPESCRIPT REST API
## Setup Typescript for dev
```
npm init -y
npm i -D typescript ts-node-dev
```
package.json
```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "ts-node-dev --respawn src/server.ts",
    "build": "tsc",
    "start":"node dist/src/server.js"
},
```
create tsconfig.json file
```
tsc --init
```
tsconfig.json file changes
```
"target": "ESNext",  
"rootDir": "./",   
"moduleResolution": "node",
"outDir": "./dist",
```

## Basic express server with typescript
```
npm i express
npm i --D @types/express
```
Create application for testing
server.ts
```
import express, { Request, Response } from "express";

const app = express();
const port = 9000;

app.get("/", (req: Request, res: Response)=>{
    res.status(200).json({status: true});
})

app.listen(port, ()=>{
    console.log(`Server is running on port : ${port}`);
})
```
## Connect to the Database
```
npm i sequelize
npm i mysql2
```
src/config/database.config.ts
```
import { Sequelize } from "sequelize";

const dbName = "mean_todos";
const dbHost = "localhost";
const dbUser = "root";
const dbPassword = "";
const dbDriver = "mysql";

const db = new Sequelize(dbName, dbUser, dbPassword, {
    host: dbHost,
    dialect: dbDriver
})

export default db;
```
server.ts
```
import express, { Request, Response } from "express";
import db from "./config/database.config";

const app = express();
const port = 9000;


// Check database connection
db.sync().then(()=>{
    console.log("Database connected successfully!");
})

app.get("/", (req: Request, res: Response)=>{
    res.status(200).json({status: true});
})

app.listen(port, ()=>{
    console.log(`Server is running on port : ${port}`);
})
```
