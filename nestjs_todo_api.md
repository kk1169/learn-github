# Create new application
```
npm i -g @nestjs/cli
nest new project-name

nest start
nest start --watch
```
## Create Module
```
nest g mo modules/quiz
nest g resource user
```
## Create Controller
```
nest g controller [name]
nest g resource [name]
```
## Add Database
```
npm install --save @nestjs/typeorm typeorm mysql2
npm install --save @nestjs/passport passport passport-local passport-jwt bcrypt @nestjs/jwt
```
## Hash Password
```
$ npm i bcrypt
$ npm i -D @types/bcrypt
```

## Class Validator
```
npm i --save class-validator class-transformer
```
