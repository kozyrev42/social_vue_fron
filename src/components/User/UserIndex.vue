<template>
  <div class="w-96 mx-auto">
    <div v-if="users">
        <div v-for="user in users" class="mb-6 pb-6 border-b border-gray-400">
            <p>{{user.name}}</p>
            <p>{{user.email}}</p>
        </div>
    </div>
  </div>
</template>

<script>
import api from "../../api";

export default {
  name: "UserIndex",

  data() {
    return {
        users:[]
    }
  },
    
    mounted() {
        this.getUsers()
    },
    
    methods: {
      getUsers() {
          api.get('/api/users')
          .then(response => {
              this.users = response.data.data
          })
      }
  }
}
</script>

<style scoped>

</style>