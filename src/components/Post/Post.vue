<template>
    <div class="mb-8 pb-8 border-b border-gray-400">
        
        <h1 class="text-xl">{{ post.title }}</h1>
        <img v-if="post.image_url" :src="post.image_url" :alt="post.title" class="my-3 mx-auto rounded-2xl">
        <p>{{ post.content}}</p>

        <!-- отображения оригинального поста у репоста, если тот имеется -->
        <div v-if="post.reposted_post" class="bg-gray-200 p-5 my-5 border border-gray-300">
            <h1 class="text-xl">{{ post.reposted_post.title }}</h1>
            <img v-if="post.reposted_post.image_url" :src="post.reposted_post.image_url" :alt="post.reposted_post.title" class="my-3 mx-auto rounded-2xl">
            <p>{{ post.reposted_post.content}}</p>
        </div>
        
        <div class="flex justify-between items-center mt-2">
            <div class="flex">
                <div class="flex mr-2" >
                    <svg @click.prevent="toggleLike(post)"
                        xmlns="http://www.w3.org/2000/svg"
                        viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"
                        :class="['mr-2 stroke-sky-500 cursor-pointer hover:fill-sky-500 w-6 h-6', post.is_liked ? 'fill-sky-500' : 'fill-white']">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M21 8.25c0-2.485-2.099-4.5-4.688-4.5-1.935 0-3.597 1.126-4.312 2.733-.715-1.607-2.377-2.733-4.313-2.733C5.1 3.75 3 5.765 3 8.25c0 7.22 9 12 9 12s9-4.78 9-12z" />
                    </svg>
    
                    <div>{{ post.likes_count}}</div>
                </div>
    
                <div class="flex">
                    <svg @click.prevent="openRepost()"
                        xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"
                         stroke-width="1.5"
                         stroke="currentColor"
                         :class="['mr-2 stroke-sky-500 cursor-pointer hover:fill-sky-500 w-6 h-6 fill-white']">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M7.217 10.907a2.25 2.25 0 100 2.186m0-2.186c.18.324.283.696.283 1.093s-.103.77-.283 1.093m0-2.186l9.566-5.314m-9.566 7.5l9.566 5.314m0 0a2.25 2.25 0 103.935 2.186 2.25 2.25 0 00-3.935-2.186zm0-12.814a2.25 2.25 0 103.933-2.185 2.25 2.25 0 00-3.933 2.185z" />
                    </svg>
    
                    <div>{{ post.reposted_by_posts_count ? post.reposted_by_posts_count : '0'}}</div>
                </div>
            </div>
            <p class="text-right text-slate-500 text-sm">{{ post.date }}</p>
        </div>
        
        <!-- выпадающая форма для репоста -->
        <div v-if="is_repost" class="mt-3">
            <div>
                <input v-model="title" class="w-96 mb-3 rounded-3xl border p-3 border-slate-300" type="text"
                       placeholder="заголовок">
            </div>
            <div>
                <textarea v-model="content" class="w-96 mb-3 rounded-3xl border p-3 border-slate-300"
                          placeholder="контент">
                </textarea>
            </div>
            
            <a @click.prevent="repost(post)" href="#" class="block p-2 w-32 text-center rounded-2xl bg-green-600 text-white
                hover:bg-white hover:border hover:border-green-600 hover:text-green-600 box-border ml-auto">
                Репост
            </a>
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

    data() {
        return {
            title:"",
            content:"",
            is_repost: false,
            repostedId: null
        }
    },
    
    methods: {
        toggleLike(post) {
            api.get(`/api/posts/${post.id}/toggle_like`)
                .then(response => {
                    post.is_liked = response.data.is_liked
                    post.likes_count = response.data.likes_count
                })
        },
        
        openRepost(){
            // если открыта страница "домашняя", то запретим открытие выпадающей форме для репоста своих постов
            if(this.$route.name === 'user.personal') return
            
            // открытие/закрытие формы для репоста
            this.is_repost = !this.is_repost;
        },
        
        repost(post){
            api.post(`/api/posts/${post.id}/repost`, {title: this.title, content: this.content})
                .then(response => {
                    this.title = ''
                    this.content = ''
                })
        }
    }
}
</script>

<style scoped>

</style>