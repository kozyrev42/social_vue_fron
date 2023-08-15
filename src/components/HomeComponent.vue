<template>
  <div>
    <div class="">HomeComponent</div> <br/>
    <router-link to="/about" class="">About</router-link>
  </div>
</template>

<script>
import api from '../api'

export default {
  name: 'HomeComponent',
  props: {
    msg: String
  },

  mounted() {
    this.testApi();
  },

  methods: {
    testApi() {
      api.get('/api/test')
        .then(response => {
          console.log(response)
        })

        .catch(error => {
          console.log(error)
        })
    },

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
