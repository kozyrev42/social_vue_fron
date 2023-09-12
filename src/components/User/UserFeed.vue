<template>
    <div class="w-96 mx-auto ">
    
        <h1 class="mb-8 pb-8 border-b border-gray-400">Посты по подпискам:</h1>
        <div v-if="posts">
            <Post v-for="post in posts" :post="post"></Post>
        </div>
        
    </div>
</template>

<script>
import api from "../../api.js";
import Post from "../Post/Post.vue";

export default {
    name: "UserFeed",
    
    data() {
        return {
            posts:[],
        }
    },
    
    components: {
        Post
    },
    
    mounted() {
        this.getFollowingPostsUser()
    },
    
    methods: {
        getFollowingPostsUser() {
            api.get(`/api/users/following_posts`)
                .then(response => {
                    this.posts = response.data.data
                })
        },
        
    }
}
</script>

<style scoped>

</style>