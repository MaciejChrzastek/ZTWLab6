<template>
    <div id="books-table">
        <table>
            <thead>
                <tr>
                    <th>Tytuł</th>
                    <th>Autor ID</th>
                    <th>Strony</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="book in booksSource" :key="book.id">
                    <BookItem v-on:delete-book="$emit('delete-book',book.id)" v-bind:book="book" v-bind:booksSource="booksSource"/>
                   
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import BookItem from '@/components/BookItem.vue'
 export default {
     name: 'books-table',
     props:{
         booksSource: Array,
     },
     components:{
         BookItem
    }
 }
</script>

<style scoped></style>

