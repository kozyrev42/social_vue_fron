<template>
    <div class="flex justify-between mb-8 text-sm text-center">
        <div>
            Подписчики
            <div>
                {{statsData.subscribers_count}}
            </div>
        </div>
        <div>
            Лайки
            <div>
                {{statsData.likes_count}}
            </div>
        </div>
        <div>
            Посты
            <div>
                {{statsData.posts_count}}
            </div>
        </div>
        <div>
            Подписки
            <div>
                {{statsData.followings_count}}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "Statistics",
    
    props:[
        'statsData'
    ]
}
</script>

<style scoped>

</style>