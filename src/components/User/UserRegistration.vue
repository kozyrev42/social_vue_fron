<template>
  <div class="w-96 mx-auto">
    <div class="">
      <input v-model="name" type="text"  class="w-96 p-1 mb-2 border border-inherit rounded-lg" placeholder="name">
      <input v-model="email" type="email"  class="w-96 p-1 mb-2 border border-inherit rounded-lg" placeholder="email">
      <input v-model="password" type="password"  class="w-96 p-1 mb-2 border border-inherit rounded-lg" placeholder="password">
      <input v-model="password_confirmation" type="password"  class="w-96 p-1 mb-2 border border-inherit rounded-lg" placeholder="confirm password">
      <div v-if="error" class="" role="alert">{{ error }}</div>
      <input @click.prevent="userRegistration" type="submit" class="block float-right mx-auto w-32 p-1 bg-sky-400 text-white rounded-lg">
    </div>
  </div>
</template>

<script>
import api from "../../api";

export default {
  name: "UserRegistration",

  data() {
    return {
      name: null,
      email: null,
      password: null,
      password_confirmation: null,

      error: null
    }
  },

  methods: {
    userRegistration() {
      api.post('/api/user/registration',
          {
            name: this.name,
            email: this.email,
            password: this.password,
            password_confirmation: this.password_confirmation
          })
        .then(response => {
          console.log(response)
          // помещаем токен в localStorage
          localStorage.setItem('social_access_token', response.data.access_token)

          // Вызываем событие успешной авторизации
          this.$emit('loginSuccess')

          // переход на страницу Home
          this.$router.push({name: 'home'})
        })

        .catch(error => {
          console.log(error)

          // в this.error помещаем сообщение об ошибке
          this.error = "формат почты неверный, или ошибка при заполнении"
        })
    }
  }
}
</script>

<style scoped>

</style>