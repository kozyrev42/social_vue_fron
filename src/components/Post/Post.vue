<template>
    <div class="mb-8 pb-8 border-b border-gray-400">
        <p class="text-right text-slate-500 text-sm">{{ post.date }}</p>
        <h1 class="text-xl">{{ post.title }}</h1>
        <img v-if="post.image_url" :src="post.image_url" :alt="post.title" class="my-3 mx-auto rounded-2xl">
        <p>{{ post.content}}</p>
    </div>
</template>

<script>
export default {
    name: "Post",
    
    props: [
        'post'
    ]
}
</script>

<style scoped>

</style>