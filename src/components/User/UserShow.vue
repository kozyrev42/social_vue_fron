<template>
    <div class="w-96 mx-auto ">
    
        <h1 class="mb-8 pb-8 border-b border-gray-400">Публикации пользователя:</h1>
        <div v-if="posts">
            <Post v-for="post in posts" :post="post"></Post>
        </div>
        
    </div>
</template>

<script>
import api from "../../api.js";
import Post from "../Post/Post.vue";

export default {
    name: "UserShow",
    
    data() {
        return {
            posts:[],
            userId: this.$route.params.id
        }
    },
    
    components: {
        Post
    },
    
    mounted() {
        this.getPostsUser()
    },
    
    methods: {
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