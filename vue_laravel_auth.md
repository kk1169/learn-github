# Install Laravel Globally
```
composer global require laravel/installer

composer create-project laravel/laravel laravel-breeze-api

laravel new laravel-breeze-api
```
Create new vue project
```
npm create vite@latest

cd vue-breeze-api
npm install
npm run dev
```
Now, install breeze
```
composer require laravel/breeze --dev

php artisan breeze:install api
```
Add below in .env
```
SANCTUM_STATEFUL_DOMAINS=localhost:3000

# like this
APP_NAME=Laravel
APP_ENV=local
APP_KEY=base64:uBzJVodD0zB8mIcyI2ZuxdJK5Eyyfzvy/t/REizp/h8=
APP_DEBUG=true
APP_URL=http://localhost:8000
FRONTEND_URL=http://localhost:3000
SANCTUM_STATEFUL_DOMAINS=localhost:3000
SESSION_DOMAIN=localhost
```
Create database and run below command
```
php artisan migrate
```
Change the port of vue js
vite.config.js
```
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [vue()],
    server: {
        port: 3000
    }
})
```
Install Below Packages
```
npm install vue-router@4
npm install pinia
npm install axios
```
Add Tailwind packages
```
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p

export default {
    content: [
        "./index.html",
        "./src/**/*.{vue,js,ts,jsx,tsx}",
    ],
    theme: {
        extend: {},
    },
    plugins: [],
}
```
