<template>
    <div id="persons-table">
        <div>
        <table>
            <thead style="border-bottom:1px solid black;">
                <!--<tr >-->
                    <!--
                    <th><pre style="font-family: Arial, Helvetica, sans-serif;"><b>     Id          Imię                       Nazwisko                       Opcje</b>                        </pre>
                        </th>-->
                        <th >
                        <td style="width:6%; border-bottom:1px solid white;"> 
                        </td>
                        <td style="width:12%; border-bottom:1px solid white;">Id 
                        </td>
                        <td style="width:14%; border-bottom:1px solid white;"> 
                        </td>
                        <td style="width:12%; border-bottom:1px solid white;">Imię
                        </td>
                        <td style="width:12%; border-bottom:1px solid white;"> 
                        </td>
                        <td style="width:16%; border-bottom:1px solid white;">Nazwisko
                        </td>
                        <td style="width:10%; border-bottom:1px solid white;"> 
                        </td>
                        <td style="width:12%; border-bottom:1px solid white;">Opcje
                        </td>
                        <td style="width:6%; border-bottom:1px solid white;"> 
                        </td>
                        </th>
               <!-- </tr>-->
            </thead>
            <tbody>
                <tr v-for="person in personsSource" :key="person.id">
                    <PersonItem v-on:delete-person="$emit('delete-person',person.id)" v-bind:person="person" v-bind:personsSource="personsSource"/>
                   <!-- <PersonItem v-on:update-person="$emit('update-person',person.id, person.name, person.surname)" v-bind:person="person" v-bind:personsSource="personsSource"/>
-->
                   <!-- <td>{{ person.name }}</td>
                    <td>{{ person.email }}</td>
                    <td>{{ person.phone }}</td> -->
                </tr>
            </tbody>
        </table>
        </div>
    </div>
</template>

<script>
import PersonItem from "@/components/PersonItem.vue";
export default {
  name: "persons-table",
  props: {
    personsSource: Array,
  },
  components: {
    PersonItem,
  },
};
</script>

<style scoped></style>

