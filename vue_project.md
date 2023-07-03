# VUEJS
```
npm i -g @vue/cli
vue --version
vue create vue-crud
```
or
```
npm init vue@latest
```
## Adding Tailwind css
```
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```
## Statement
```
<li v-for="(item, index) in items">
  {{ parentMessage }} - {{ index }} - {{ item.message }}
</li>
```