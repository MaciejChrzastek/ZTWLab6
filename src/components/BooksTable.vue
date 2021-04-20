<template>
    <div id="books-table">
        <table>
            <thead>
                 <th >
                        
                        <td style="width:10%; border-bottom:1px solid white;">Tytuł
                        </td>
                        <td style="width:4%; border-bottom:1px solid white;"> 
                        </td>
                        <td style="width:10%; border-bottom:1px solid white;">Imię Autora
                        </td>
                        <td style="width:4%; border-bottom:1px solid white;"> 
                        </td>
                        <td style="width:10%; border-bottom:1px solid white;">Nazwisko Autora
                        </td>
                        <td style="width:2%; border-bottom:1px solid white;"> 
                        </td>
                        <td style="width:10%; border-bottom:1px solid white;">Id Autora
                        </td>
                        <td style="width:4%; border-bottom:1px solid white;"> 
                        </td>
                        <td style="width:10%; border-bottom:1px solid white;">L. Stron
                        </td>
                        <td style="width:3%; border-bottom:1px solid white;"> 
                        </td>
                        <td style="width:12%; border-bottom:1px solid white;">Opcje
                        </td>
                        <td style="width:5%; border-bottom:1px solid white;"> 
                        </td>
                        </th>
                
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

