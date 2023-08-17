<template>

</template>

<script>
import api from '../api'

export default {
  name: 'HomeComponent',

  data() {
    return {
      accessToken: null,
      refreshingToken: false // Флаг для отслеживания состояния обновления токена
    }
  },


  mounted() {
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


  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
