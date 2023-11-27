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

8. Создал страницу с отбражением всех пользователей. Получение пользователей с сервера.

9. Получение и отображение постов на странице пользователей. Пост вынес в отдельный компонент.

10. Кнопка 'Отписаться/Подписаться', отправка на сервер ID, для смены статуса

11. Лента новостей. Сделал страницу с постами, тех юзеров, на которых подписан аутентифицированный

12. Верстка Лайка поста на странице "домашняя", запрос на сервер для переключения

13. Функционал Репоста (без подсчета количества репостов текущего поста).
    - кнопка и метод для открытия формы репоста
    - форма для создания/заполнения репоста
    - метод для отправки на сервер репоста
    - верстка отображения оригинального поста у репоста

14. Вывод подсчета количества репостов текущего поста.

15. Функционал Комментариев.
    - форма создания комментария
    - отображение списка комментариев у поста
    - метод для отправки данных для создания комментария
    - метод для получения с сервера комментариев
    - показ/скрытие списка комментариев

16. Обработка ошибок при создании поста. Отрисовка кнопки "отмена", только если изображение предзагружено

17. Отображение автора поста и репоста. Автор поста является ссылкой.

18. Статистика по юзеру. Создан компонент для вывода статистики. Запрос на сервер для получения статистики.

19. Поправил вёрстку, в роутинге добавил правило