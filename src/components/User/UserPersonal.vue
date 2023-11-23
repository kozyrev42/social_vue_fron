<template>
    <!-- страница "домашняя" -->
    <div class="w-96 mx-auto ">
        <Statistics :statsData="stats"></Statistics>
        <div class="mb-4">
            <div class="mb-3">
                <input v-model="title" class="w-96 rounded-3xl border p-3 border-slate-300" type="text"
                       placeholder="заголовок">
                <!-- обработка ошибок -->
                <div v-if="errors.title">
                    <p v-for="error in errors.title" class="text-red-600">Поле "заголовок" не заполнено</p>
                </div>
                
            </div>
            <div class="mb-3">
                <textarea v-model="content" class="w-96 rounded-3xl border p-3 border-slate-300"
                          placeholder="контент"></textarea>
                <!-- обработка ошибок -->
                <div v-if="errors.content">
                    <p v-for="error in errors.content" class="text-red-600">Поле "контент" не заполнено</p>
                </div>
            </div>
            
            <div class="flex mb-3 items-center">
                <div>
                    <!-- После выбора файла, срабатывает событие change, и вызывается метод storeImage -->
                    <input @change="storeImage" ref="file" type="file" class="hidden"/>
    
                    <!-- При клике на ссылку, открытие диалогового окна выбора файла -->
                    <a href="#" class="block p-2 w-32 text-center text-sm rounded-2xl bg-sky-500 text-white
                    hover:bg-white hover:border hover:border-sky-500 hover:text-sky-500 box-border"
                       @click.prevent="selectFile()">
                        Изображение
                    </a>
                </div>
                
                <div>
                    <!-- .prevent - чтобы ссылка "#" не дёргалась -->
                    <!-- отрисовка кнопки "отмена", только если изображение предзагружено -->
                    <a v-if="image" @click.prevent="image = null" href="#" class="ml-3">
                        Отмена
                    </a>
                </div>
            </div>
            
            <div v-if="image" class="mb-3">
                <!-- браузер загрузит и отобразит изображение напрямую с сервера -->
                <img class="rounded-2xl" :src="image.url" alt="preview">
            </div>
            
            <div>
                <a @click.prevent="createPost" href="#" class="block p-2 w-32 text-center rounded-2xl bg-green-600 text-white
          hover:bg-white hover:border hover:border-green-600 hover:text-green-600 box-border ml-auto">
                    Опубликовать
                </a>
            </div>
        </div>
        
        <div v-if="posts">
            <h1 class="mb-8 pb-8 border-b border-gray-400">Мои публикации:</h1>
            <Post v-for="post in posts" :post="post"></Post>
        </div>
    </div>
</template>

<script>
import api from "../../api.js";
import Post from "../Post/Post.vue";
import Statistics from "../Statistics/Statistics.vue";

export default {
    name: "UserPersonal",
    
    data() {
        return {
            title: '',
            content: '',
            image: null,
            posts:[],
            errors: [],
            stats: []
        }
    },
    
    components: {
        Post,
        // подключаем компонент "статистика"
        Statistics
    },
    
    mounted() {
        this.getPostsAuth()
        this.getStatistics()
    },
    
    methods: {
        
        getStatistics() {
            api.post('/api/users/stats', {id: null})
                .then(response => {
                    this.stats = response.data.data
                })
        },
        
        getPostsAuth() {
            api.get('/api/posts/auth')
                .then(response => {
                    this.posts = response.data.data
                })
        },
        
        createPost() {
            const image_id = this.image ? this.image.id : null
            api.post('/api/post/create', {title: this.title, content: this.content, image_id: image_id})
                .then(response => {
                    this.title = ''
                    this.content = ''
                    this.image = null
                    // добавляем новосозданный пост в начало массива posts. Новый пост будет отображаться вверху списка
                    this.posts.unshift(response.data.data)
                })
                .catch( e => {
                        // ошибки помещаем в подготовленный массив errors
                        this.errors = e.response.data.errors
                    }
                )
        },
        
        // цель этого метода - это обеспечить пользователю возможность выбора файла,
        // не делая на самом деле видимым элемент <input type="file">.
        selectFile() {
            // получаем доступ к скрытому элементу
            this.fileInput = this.$refs.file
            
            // события клика на элементе <input ref="file" type="file">
            // открывается стандартное диалоговое окно для выбора файла
            this.fileInput.click()
        },
        
        storeImage(e) {
            // получаем выбранный пользователем файл
            const file = e.target.files[0]
            
            // создаем новый объект FormData, который позволяет отправить файлы через AJAX-запрос.
            const formData = new FormData()
            
            // добавляем выбранный файл в объект FormData под именем image.
            formData.append('image', file)
            
            // отправляет файл на сервер
            api.post('/api/post/image', formData)
                .then(response => {
                    // сохраняете данные ответа в переменную "image"
                    this.image = response.data.data
                })
        }
    }
}
</script>

<style scoped>

</style>