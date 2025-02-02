<script setup>
import { ref, reactive } from 'vue'

const props = defineProps({
    book: Object
})

const dialog = ref(false)

const postItem = reactive({
    book: {}
})

const items = [
    {
        avatar: 'https://cdn.vuetifyjs.com/images/lists/1.jpg',
        title: 'Brunch this weekend?',
        subtitle: `<span class="text--primary">Ali Connors</span> &mdash; I'll be in your neighborhood doing errands this weekend. Do you want to hang out?`,
    },
    { divider: true, inset: true },
    {
        avatar: 'https://cdn.vuetifyjs.com/images/lists/2.jpg',
        title: 'Summer BBQ <span class="grey--text text--lighten-1">4</span>',
        subtitle: `<span class="text--primary">to Alex, Scott, Jennifer</span> &mdash; Wish I could come, but I'm out of town this weekend.`,
    },
    { divider: true, inset: true },
    {
        avatar: 'https://cdn.vuetifyjs.com/images/lists/3.jpg',
        title: 'Oui oui',
        subtitle: '<span class="text--primary">Sandra Adams</span> &mdash; Do you have Paris recommendations? Have you ever been?',
    },
    { divider: true, inset: true },
    {
        avatar: 'https://cdn.vuetifyjs.com/images/lists/4.jpg',
        title: 'Birthday gift',
        subtitle: '<span class="text--primary">Trevor Hansen</span> &mdash; Have any ideas about what we should get Heidi for her birthday?',
    },
    { divider: true, inset: true },
    {
        avatar: 'https://cdn.vuetifyjs.com/images/lists/5.jpg',
        title: 'Recipe to try',
        subtitle: '<span class="text--primary">Britta Holt</span> &mdash; We should eat this: Grate, Squash, Corn, and tomatillo Tacos.',
    },
]

// 本詳細ダイアログオープン関数
const openDialog = (book) => {
    dialog.value = true;
    postItem.book = book;
}

// 本詳細ダイアログclose関数
const closeDialog = () => {
    dialog.value = false;
}

</script>

<template>
    <div>
        <v-card class="mx-auto" elevation="2" max-width="700px" @click=openDialog(book)>
            <v-card-actions>
                <v-container class="grey lighten-5">
                    <v-row :align="align">
                        <v-col cols="2">
                            <v-img width="100" v-bind:src="book.volumeInfo.imageLinks.smallThumbnail"></v-img>
                        </v-col>
                        <v-col cols="10">
                            <v-row>
                                <v-card-title class="text-h5">
                                    {{ book.volumeInfo.title }}
                                </v-card-title>
                            </v-row>
                            <v-row>
                                <v-card-text>
                                    <v-list lines="one">
                                        <v-list-item title="貸出ステータス">
                                            <v-icon x-small :color="book.status ? 'green' : 'red' + ' darken-2'">
                                                mdi-moon-full
                                            </v-icon>
                                        </v-list-item>
                                        <v-list-item title="場所" :subtitle="book.location"></v-list-item>
                                    </v-list>
                                </v-card-text>
                            </v-row>
                        </v-col>
                    </v-row>
                </v-container>
            </v-card-actions>

            <v-card-actions>
                <v-btn elevation="2">借りる</v-btn>
            </v-card-actions>
        </v-card>

        <!-- dialog -->
        <v-dialog v-model="dialog" max-width="500px" max-height="600px">
            <v-card class="mx-auto">
                <v-card-title>
                    <span>書籍詳細</span>
                </v-card-title>

                <v-row>
                    <v-col>
                        <v-img class="ml-auto my-auto" max-height="200" max-width="200"
                            :src="postItem.book.volumeInfo.imageLinks.smallThumbnail">
                        </v-img>
                    </v-col>
                    <v-col class="my-auto">
                        <v-card-text>
                            名前： {{ postItem.book.volumeInfo.title }}<br>
                            ステータス： {{ postItem.book.status ? "貸出可能" : "貸出不可" }}
                            <v-icon x-small :color="postItem.book.status ? 'green' : 'red' + ' darken-2'">
                                mdi-moon-full
                            </v-icon><br>
                            在庫数： 2 冊
                        </v-card-text>
                    </v-col>
                </v-row>

                <v-list class="my-3" max-height="200">
                    <template v-for="(item, index) in items">
                        <v-divider v-if="item.divider" :key="index" :inset="item.inset"></v-divider>

                        <v-list-item v-else :key="item.title">
                            <v-list-item-avatar>
                                <v-img :src="item.avatar" max-height="50" max-width="50"></v-img>
                            </v-list-item-avatar>

                            <v-list-item-content>
                                <v-list-item-title v-html="item.title"></v-list-item-title>
                                <v-list-item-subtitle v-html="item.subtitle"></v-list-item-subtitle>
                            </v-list-item-content>
                        </v-list-item>
                    </template>
                </v-list>

                <v-divider></v-divider>
                <v-card-actions justify='center'>
                    <v-col cols="6">
                        <v-btn @click="closeDialog" color="blue darken-1">閉じる</v-btn>
                    </v-col>
                    <v-col cols="6">
                        <v-btn @click="closeDialog" color="blue darken-1">借りる</v-btn>
                    </v-col>
                </v-card-actions>

            </v-card>
        </v-dialog>

    </div>
</template>

