# Sequelize Notes
Create node application
```
npm init -y
npm i express sequelize mysql2
npm i -D nodemon
```
Create App using express
```
const express = require('express')
const app = express()
const port = 3000

app.use(express.json());

app.get('/', (req, res) => {
    res.send('Hello World!')
})

app.listen(port, () => {
    console.log(`Example app listening on port ${port}`)
})
```
## Sequelize Mysql Connection
models/index.js
```
const { Sequelize } = require('sequelize');

const db = new Sequelize('nodejs_sequelize', 'root', '', {
    host: 'localhost',
    dialect: 'mysql'
});


// Test Database Connection
try {
    db.authenticate();
    console.log('Connection has been established successfully.');
} catch (error) {
    console.error('Unable to connect to the database:', error);
}

module.exports = db;
```