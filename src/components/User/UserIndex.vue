<template>
  <div class="w-96 mx-auto">
    <div v-if="users">
        <div v-for="user in users" class="flex justify-between mb-6 pb-6 border-b border-gray-400">
            <router-link :to="{name: 'user.show', params: {id: user.id}}">
                <p>{{user.name}}</p>
                <p>{{user.email}}</p>
            </router-link>
            
            <div>
                <a @click.prevent="toggleFollowing(user)"
                   href="#"
                   class="block p-2 w-32 text-center text-sm rounded-2xl"
                   :class="user.is_followed ? 'border border-sky-500 bg-white text-sky-500' : 'bg-sky-500 text-white'">
                    {{ user.is_followed ? 'Отписаться' : 'Подписаться' }}
                </a>

            </div>
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
      },
        
        toggleFollowing(user) {
            api.get(`/api/users/${user.id}/toggle_following`)
                .then(response => {
                    user.is_followed = response.data.is_followed
                })
        }
  }
}
</script>

<style scoped>

</style>