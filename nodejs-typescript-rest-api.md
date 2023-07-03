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
## Create Model
src/models/Todo.ts
```
import { DataTypes, Model } from 'sequelize';
import db from '../config/database.config';

interface TodoAttributes{
    id: number;
    title: string;
    description: string;
    status: number;
    createdAt?: Date;
    updatedAt?: Date;
    deletedAt?: Date;
}

export class Todo extends Model<TodoAttributes>{}

Todo.init(
    {
        id:{
            type: DataTypes.INTEGER.UNSIGNED,
            autoIncrement: true,
            primaryKey: true
        },
        title:{
            type: DataTypes.STRING,
            allowNull: false
        },
        description:{
            type: DataTypes.TEXT
        },
        status:{
            type: DataTypes.INTEGER
        }
    },
    {
        timestamps: true,
        tableName: "todos",
        paranoid: true,
        sequelize: db
    }
)
```
## Create todo
controllers/TodoController.ts
```
import { Request, RequestHandler, Response } from "express";
import { Todo } from "../models/Todo";
import { todoValidation } from "../validators";


class TodoController{

    async getAllTodo(req: Request, res: Response) {
        try {
            const todos = await Todo.findAll();
            console.log(todos);
    
            return res.json({ status: true, data: todos });
        } catch (error) {
            return res.json({ status: false, error })
        }
    }

    async getTodoPagination(req: Request, res: Response){
        try {
    
            const limit = Number(req.query?.limit as number | undefined);
            const offset = Number(req.query?.offset as number | undefined);
    
            const todos = await Todo.findAll({ where: {}, limit, offset });
    
            return res.json({ status: true, data: todos });
        } catch (error) {
            return res.json({ status: false, error })
        }
    }

    async getTodo(req: Request, res: Response){
        try {
            const todo = await Todo.findByPk(req.params.id);
            return res.json({ status: true, data: todo });
        } catch (error) {
            return res.json({ status: false, error })
        }
    }

    async storeTodo(req: Request, res: Response){
        const { error, value } = todoValidation.validate(req.body);
    
        if (error) {
            return res.json({ status: false, error: error.details });
        }
    
        try {
    
            const todo = Todo.create({ ...req.body });
            return res.json({ status: true, message: 'Item created successfully!', data: todo });
        } catch (error) {
            return res.json({ status: false, message: 'Item created unsuccessfull!' })
        }
    }

    async updateTodo(req: Request, res: Response){
        try {
    
            const {id} = req.params;
            const todo = await Todo.findOne({where: {id}})
    
            await todo?.update({...req.body});
    
            return res.json({ status: true, message: 'Item updated successfully!', data: todo });
        } catch (error) {
            return res.json({ status: false, message: 'Item updated unsuccessfull!' })
        }
    }

    async deleteTodo(req: Request, res: Response){
        try {
            const {id} = req.params;
            const todo = await Todo.findOne({where: {id}});
    
            await todo?.destroy()
    
            return res.json({ status: true, message: 'Item deleted successfully!', data: todo });
        } catch (error) {
            return res.json({ status: false, message: 'Item deleted unsuccessfull!' })
        }
    }
}

export default new TodoController();
```
routes/api.routes.ts
```
import { Router } from "express";
import TodoController from './../controllers/TodoController';
export const apiRouter = Router();

apiRouter.get("/todo", TodoController.getAllTodo);
apiRouter.get("/todo/pagination", TodoController.getTodoPagination);
apiRouter.get("/todo/:id", TodoController.getTodo);
apiRouter.post("/todo", TodoController.storeTodo);
apiRouter.put("/todo/:id", TodoController.updateTodo);
apiRouter.delete("/todo/:id", TodoController.deleteTodo);
```
server.ts
```
import express, { Request, Response } from "express";
import db from "./config/database.config";
import { Todo } from "./models/Todo";
import { apiRouter } from "./routes/api.routes";

const app = express();
const port = 9000;

app.use(express.json());

// Check database connection
db.sync().then(()=>{
    console.log("Database connected successfully!");
})

app.get("/", (req: Request, res: Response)=>{
    res.status(200).json({status: true});
})

app.use("/api", apiRouter)

app.listen(port, ()=>{
    console.log(`Server is running on port : ${port}`);
})
```

# JWT Token
Install Packages
```
npm install --save @types/bcrypt
npm i dotenv
```