## Create Vue Project
```
Todo Application
```

## Setup Store in Vue
```
npm i pinia
```
### main.js
```
import { createPinia } from 'pinia'
const app = createApp(App)
app.use(createPinia())
```