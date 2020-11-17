<template>
    <div id="app">
        <input type="text" v-model = "id">
        <button v-on:click="getPostsById">Найти пост</button>
        <button @click = "toggleSendForm">Создать пост</button>
             <div v-if="showSendForm">
                 <input type="text" placeholder="Заголовок" v-model="post.title"><br>
                 <textarea v-model="post.body"></textarea><br>
                 <button @click="sendData">Отправить</button>
             </div>
        {{ notes.title }} {{ notes.body }}
             <div v-if="showAll">
                 <ul>
                     <li v-for = "note in notes">
                         {{ note.title }}<br>
                         {{ note.body }}<br>
                     </li>
                </ul>
             </div>
    </div>
</template>

<script>
    import Vue from 'vue'
    import axios from 'axios'
    import VueAxios from 'vue-axios'
    Vue.use(VueAxios, axios)
    export default {
        data(){
            return {
                post: {
                    "title":null,
                    "body": null
                },
                id: null,
                notes: {},
                showAll: true,
                showSendForm: false
            }
        },
        mounted() {
            this.getPosts()
        },
        methods: {
            getPosts() {
                axios.get('/api/notes')
                .then ( results=> {
                    (this.notes = results.data)
                })
                },
            getPostsById() {
                this.showAll = false
                axios.get(`/api/notes/${this.id}`)
                .then ( results => {
                    (this.notes = results.data)
                })
                },
            toggleSendForm() {
                this.showSendForm = !this.showSendForm
            },
            sendData() {
                axios.post('/api/notes', this.post)
                .then ( res=> {
                    (console.log('done'))
                })
            }
            }
        }
</script>
