<script setup>
import { ref } from 'vue'
// import books from '../assets/bookData'
import BookListItem from '@/components/BookListItem.vue';
import useSWRV from 'swrv'

const searchString = ref("")

const fetcher = (search) => {
    // const search = "cicd"
    const url = new URL("https://www.googleapis.com/books/v1/volumes")

    const param = {
        q: search
    }


    url.search = new URLSearchParams(param).toString()
    return fetch(url)
        .then((res) => {
            return res && res.json()
        }).then((data) => {
            return data
        })
}

const { data, error } = useSWRV("docker", fetcher)
console.log(data)

</script>

<template>
    <v-container>
        <!-- Header -->
        <v-row>
            <v-col>
                <h1>社内図書システム</h1>
            </v-col>
            <v-col>
                <router-link to="/foo">書籍詳細ダイアログ</router-link>|
                <router-link to="/UserDetail">UserDetail</router-link>|
                <router-link to="/">本一覧画面</router-link>
            </v-col>
            <v-col>
                <router-link to="/UserDetail">
                    <v-avatar size="56">
                        <v-img src="../assets/kanako.png"></v-img>
                    </v-avatar>
                </router-link>
            </v-col>
        </v-row>

        <!-- 検索フォーム -->
        <v-form>
            <v-text-field v-model="searchString" prepend-inner-icon="mdi-magnify" label="書籍検索" placeholder="書籍名等を入力してください"
                clear-icon="mdi-close-circle" clearable></v-text-field>
        </v-form>

        <!-- 本一覧 -->
        <div v-if="error">failed to load</div>
        <div v-if="!data">loading...</div>
        <div v-else>
            <div v-for="book in data.items" :key="book">
                <BookListItem :book=book />
            </div>
        </div>

    </v-container>
</template>

