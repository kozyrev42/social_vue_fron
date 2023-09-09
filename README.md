# Vue 3 + Vite

This template should help get you started developing with Vue 3 in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run dev
```

1) проект vue + vite
>npm init vite@latest social_vue_fron -- --template vue

// Установите tailwindcss и его одноранговые зависимости:
>npm install -D tailwindcss postcss autoprefixer

// создайте файлы tailwind.config.js и postcss.config.js:
>npx tailwindcss init -p

// настроил tailwind.config.js

2) Установка Vue Router, плюс 2 проверочных компонента
npm install vue-router@4

3) Подключил axios в файле: "src/api.js"
>npm install axios
+ отправил тестовый запрос к api

4) Добавил компоненты для авторизации, регистрации,
и получения списка фруктов, поправил роутинг с учетом vite

5) вынес кнопки из роутинги, почистил HomeComponent, добавил роут personal, вывод ошибки при регистрации

6) Сделал форму для создания Поста + выбора изображения;
- при выборе изображения из файловаой системы, изображение загружается на сервер 
- и тут же отрисовывается(загружается) с сервака, получаем ID изображения

-- При отправке поста на сервер, к посту прикрепляем ID изображения

7. Запрос на сервер "за публикациями пользователя" + отображение публикация в цикле