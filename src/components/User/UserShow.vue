<template>
    <!-- страница "пользователи -> пользователь" -->
    <div class="w-96 mx-auto ">
        <Statistics :statsData="stats"></Statistics>
        <h1 class="mb-8 pb-8 border-b border-gray-400">Публикации пользователя:</h1>
        <div v-if="posts">
            <Post v-for="post in posts" :post="post"></Post>
        </div>
        
    </div>
</template>

<script>
import api from "../../api.js";
import Post from "../Post/Post.vue";
import Statistics from "../Statistics/Statistics.vue";

export default {
    name: "UserShow",
    
    data() {
        return {
            posts:[],
            // в userId - прилетит id из урла-браузера, потому-что в роутинге мы прописали :id '/user/:id/show'
            userId: this.$route.params.id,
    
            stats: []
        }
    },
    
    components: {
        Post,
        // подключаем компонент "статистика"
        Statistics
    },
    
    mounted() {
        this.getPostsUser()
        this.getStatistics()
    },
    
    methods: {
        getStatistics() {
            api.post('/api/users/stats', {user_id: this.userId})
                .then(response => {
                    this.stats = response.data.data
                })
        },
        
        getPostsUser() {
            api.get(`/api/users/${this.userId}/posts`)
                .then(response => {
                    this.posts = response.data.data
                })
        },
        
    }
}
</script>

<style scoped>

</style>