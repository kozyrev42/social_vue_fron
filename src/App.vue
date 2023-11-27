<template>
    <div class="flex justify-between p-8 w-96 mx-auto">
      <router-link v-if="!accessToken" to="/user/login" class="">Login</router-link>
      <router-link v-if="!accessToken" to="/user/registration" class="">Registration</router-link>
      <router-link v-if="accessToken" to="/user/index" class="">Пользователи</router-link>
      <router-link v-if="accessToken" to="/user/feed" class="">Лента</router-link>
      <router-link v-if="accessToken" to="/user/personal" class="">Домашняя</router-link>
      <button v-if="accessToken" @click.prevent="logout" class="">Выход</button>
    </div>

  <router-view @loginSuccess="updateAccessToken"></router-view>
</template>

<script>
import api from './api'

export default {
  name: "App",
  data() {
    return {
      accessToken: null,
      refreshingToken: false // Флаг для отслеживания состояния обновления токена
    }
  },
  props: {
    msg: String
  },

  mounted() {
    this.testApi();
    this.getAccessToken();
  },


  // обновляем статус токена после обновления компонента
  updated() {
    this.getAccessToken();
  },


  methods: {
    // получение токена, из локального хранилища
    getAccessToken() {
      this.accessToken = localStorage.getItem('social_access_token');
    },


    updateAccessToken() {
      this.getAccessToken(); // Обновление accessToken из localStorage
    },

    testApi() {
      api.get('/api/test')
        .then(response => {
          console.log(response)
        })

        .catch(error => {
          console.log(error)
        })
    },

    // метод выхода из системы
    logout() {
      api.post('/api/auth/logout')
        .then(response => {
          console.log(response)
          // удаляем токен из localStorage
          localStorage.removeItem('social_access_token');

          // переход на страницу user.login
          this.$router.push({name: 'user.login'})

          // Обновляем компонент
          this.$forceUpdate();
        })

        .catch(error => {
          console.log(error)

          // Проверяем, была ли ошибка "401 Unauthorized"
          if (error.response && error.response.status === 401) {
            if (!this.refreshingToken) { // Проверяем, не выполняется ли уже обновление
              this.refreshingToken = true; // Устанавливаем флаг, что обновление началось

              // Пытаемся обновить токен
              api.post('/api/auth/refresh')
                .then(refreshResponse => {
                  // Получаем новый токен из ответа
                  const newAccessToken = refreshResponse.data.access_token;

                  // Обновляем токен в localStorage
                  localStorage.setItem('social_access_token', newAccessToken);

                  // Повторно пытаемся выполнить запрос на выход
                  this.logout();
                })
                .catch(refreshError => {
                  console.log(refreshError);
                })
                .finally(() => {
                  this.refreshingToken = false; // Снимаем флаг после завершения обновления
                });
            }
          }
        })
    }
  }
}
</script>

<style scoped>
    .mx-auto {
        padding-left: 8px;
        padding-right: 8px;
    }
</style>
