<template>
    <div id="persons-table">
        <table>
            <thead>
                <tr>
                    <th>Imię i nazwisko</th>
                    <th>email</th>
                    <th>telefon</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="person in personsSource" :key="person.id">
                    <PersonItem v-on:delete-person="$emit('delete-person',person.id)" v-bind:person="person" v-bind:personsSource="personsSource"/>
                   <!-- <td>{{ person.name }}</td>
                    <td>{{ person.email }}</td>
                    <td>{{ person.phone }}</td> -->
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import PersonItem from '@/components/PersonItem.vue'
 export default {
     name: 'persons-table',
     props:{
         personsSource: Array,
     },
     components:{
         PersonItem
    }
 }
</script>

<style scoped></style>

