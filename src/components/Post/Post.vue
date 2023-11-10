<template>
    <div class="mb-8 pb-8 border-b border-gray-400">
        
        <h1 class="text-xl">{{ post.title }}</h1>
        <img v-if="post.image_url" :src="post.image_url" :alt="post.title" class="my-3 mx-auto rounded-2xl">
        <p>{{ post.content}}</p>

        <div class="flex justify-between items-center mt-2">
            <div class="flex">
                <svg @click.prevent="toggleLike(post)"
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" 
                    :class="['mr-2 stroke-sky-500 cursor-pointer hover:fill-sky-500 w-6 h-6', post.is_liked ? 'fill-sky-500' : 'fill-white']">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M21 8.25c0-2.485-2.099-4.5-4.688-4.5-1.935 0-3.597 1.126-4.312 2.733-.715-1.607-2.377-2.733-4.313-2.733C5.1 3.75 3 5.765 3 8.25c0 7.22 9 12 9 12s9-4.78 9-12z" />
                </svg>

                <div>{{ post.likes_count}}</div>
            </div>

            <p class="text-right text-slate-500 text-sm">{{ post.date }}</p>
        </div>
    </div>
</template>

<script>
import api from "../../api";

export default {
    name: "Post",
    
    props: [
        'post'
    ],

    methods: {
        toggleLike(post) {
            api.get(`/api/posts/${post.id}/toggle_like`)
                .then(response => {
                    post.is_liked = response.data.is_liked
                    post.likes_count = response.data.likes_count
                })
        }
    }
}
</script>

<style scoped>

</style>