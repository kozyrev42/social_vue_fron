<template>
  <div class="w-96 mx-auto">

    <div class="w-50">
      <input v-model="email" type="email" id="email" class="w-80 p-1 mb-2 border border-inherit rounded-lg" placeholder="email">
      <input v-model="password" type="password" id="password" class="w-80 p-1 mb-2 border border-inherit rounded-lg" placeholder="password">

      <div v-if="error" class="" role="alert">{{ error }}</div>

      <input @click.prevent="login" type="submit" class="block float-right mx-auto w-32 p-1 bg-sky-400 text-white rounded-lg">
    </div>
  </div>
</template>

<script>
import api from "../../api";

export default {
  name: "UserLogin",

  data() {
    return {
      email: null,
      password: null,
      error: null
    }
  },

  methods: {
    login() {
      api.post('/api/auth/login',
          {
            email: this.email,
            password: this.password
          })
        .then(response => {
          // помещаем токен в localStorage
          localStorage.setItem('social_access_token', response.data.access_token)

          // Вызываем событие успешной авторизации
          this.$emit('loginSuccess');

          // переход на страницу Home
          this.$router.push({name: 'home'})
        })

        .catch(error => {
          console.log(error)
          // в this.error помещаем сообщение об ошибке
          this.error = error.response.data.error
        })
    }
  }
}
</script>

<style scoped>

</style>