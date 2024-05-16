# 1- vuejs projekt létrehozása CMD-adminisztrátorként
1. mappa létrehozása, pl.: ```c:\vue-gyak```
2. belépés a mappába ```cd c:\vue-gyak```
3. parancssor indítása a mappában (adminként)
4. ``` npm init vue@latest ```
5. Ok to proceed? (y) ```y```
6. Projekt név megadása, pl: √ Project name: ... ```DrinkFrontend```
- √ Package name: ... drinkfrontend
7. NO-√ Add TypeScript? ... ```No``` / Yes
8. NO-√ Add JSX Support? ... ```No``` / Yes
9. YES-√ Add Vue Router for Single Page Application development? ... No / ```Yes```
10. NO-√ Add Pinia for state management? ... ```No``` / Yes
11. NO-√ Add Vitest for Unit Testing? ... ```No``` / Yes
12. NO-√ Add an End-to-End Testing Solution? » ```No```
13. NO-√ Add ESLint for code quality? ... ```No``` / Yes
14. NO-√ Add Vue DevTools 7 extension for debugging? (experimental) ... ```No``` / Yes
15. ```cd DrinkFrontend```
16. ```npm i bootstrap vee-validate yup axios```

# 2- fejlesztő környezet megnyitása
17. jobb klikk a mappán, open folder as php storm project

# 3- Projekt futtatása
18. ```npm run dev```
19. Megnyitás böngészőben: ```http://localhost:5173/```

# 4- dolgok törlése, átalakítása
20. ```c:\vue-gyak\DrinkFrontend\src\views\AboutView.vue``` - törlés
21. ```c:\vue-gyak\DrinkFrontend\src\App.vue```
    <script setup>
    import { RouterLink, RouterView } from 'vue-router'
    </script>
    
    <template>
      <RouterView />
    </template>
    
    <style scoped>
    </style>
22. ```c:\vue-gyak\DrinkFrontend\src\components\``` - mappa (csak a) tartalmának a törlése
23. ```c:\vue-gyak\DrinkFrontend\src\assets\``` - mappa törlése (teljes mappa)
24. ```c:\vue-gyak\DrinkFrontend\src\main.js```
```
    //import './assets/main.css' - ez a sor nem kell!!!!
    
    import { createApp } from 'vue'
    import App from './App.vue'
    import router from './router'
    
    const app = createApp(App)
    
    app.use(router)
    
    app.mount('#app')
```
25. ```c:\vue-gyak\DrinkFrontend\src\views\HomeView.vue```
```
    <script setup>
    </script>
    
    <template>
    </template>
```
26. ```c:\vue-gyak\DrinkFrontend\src\router\index.js```
-kiszedni az AboutView.vue hivatkozást (//-el jelölve, mi nem kell!!!!)!!!!
```
    import { createRouter, createWebHistory } from 'vue-router'
    import HomeView from '../views/HomeView.vue'
    
    const router = createRouter({
      history: createWebHistory(import.meta.env.BASE_URL),
      routes: [
        {
          path: '/',
          name: 'home',
          component: HomeView
        }//,
        //{
        //  path: '/about',
        //  name: 'about',
        //  // route level code-splitting
        //  // this generates a separate chunk (About.[hash].js) for this route
        //  // which is lazy-loaded when the route is visited.
        //  component: () => import('../views/AboutView.vue')
        //}
      ]
    })
```    
    export default router
27. ```c:\vue-gyak\DrinkFrontend\src\main.js```
-Bele kell tenni a bootstrap hivatkozásokat!!!
```
    import { createApp } from 'vue'
    import App from './App.vue'
    import router from './router'
      import 'bootstrap'
      import 'bootstrap/dist/css/bootstrap.min.css'
    
    const app = createApp(App)
    
    app.use(router)
    
    app.mount('#app')
```
28. ```c:\vue-gyak\DrinkFrontend\src\utils\http.js```
-'utils' mappa és benne a 'http.js' fájl létrehozása!
29. ```c:\vue-gyak\DrinkFrontend\src\utils\http.js```
- ```http.js``` - adattal feltöltése:
```
    import axios from "axios";
    const http = axios.create({
        baseURL:"http://127.0.0.1:8000/api/"
    })
    export {http}
```
# 5- backend install
30. ``` start menü + xampp contorlpanel ``` indítsd el az APACHE + MySQL
31. ```c:\vue-gyak\drinks_backend\``` - másold fel a backend mappa tartalmát
32. ```composer install``` - nyiss egy cli-t admin módban, és add ki a parancsot, hogy a függőségek települjenek!
33. ```php artisan migrate``` - készüljön el az adatbázis!
34. ```php artisan db:seed``` - Töltődjenek fel a táblák adattal!!!!
35. ```php artisan serve ``` - indítsd el a szervert!
36. ```http://127.0.0.1:8000/api/drinks``` - Ezen a címen egy böngészőből elérhető a backend (elvileg)
37. Ne zárd be a CLI-t!

# 6- Frontend megírása - kezdjük a HomeView.vue-val!

38. Írjuk meg a ```HomeView.vue```-fájlt!
```
<script setup>
import {http} from '@/utils/http';
import {reactive, onMounted, ref} from 'vue';
import {RouterLink} from 'vue-router';

const drinks = reactive([]);
async function getData(){
  const response = await http.get('drinks')
  for (const item of response.data.data){
    const obj = {
      'id' :item.id,
      'name' : item.name,
      'price' : item.price,
      'discounted' : discount(item.discounted)
    }
    drinks.push(obj)
  }
}

function discount(data){
  if (data === true){
    return "Akciós"
  }
  else {
    return "Nem akciós"
  }
}
onMounted(getData)
</script>

<template>
<main>
  <h1>Italok</h1>
  <hr>
  <table class="table table-responsive">
    <thead>
      <tr>
        <th>Név</th>
        <th>Ár</th>
        <th>Akciós</th>
        <th>Művelet</th>
      </tr>
    </thead>
    <tbody>
    <tr v-for="drink in drinks" :key="drink.id">
      <td>{{drink.name}}</td>
      <td>{{drink.price}}</td>
      <td>{{drink.discounted}}</td>
      <td><router-link class="btn btn-primary" :to="`/drinks/${drink.id}`">Megjelenítés</router-link></td>
    </tr>
    </tbody>
  </table>
</main>
</template>
```
