<template>
  <div id="app" class="small-container">
    <h1 style="text-align: center">Książki i Autorzy</h1>
    <p></p>
    <h2 style="text-align: center; background-color: rgb(193, 240, 252)">
      <hr style="border-color: white" />
      Książki
      <hr style="border-color: white" />
    </h2>
    <p></p>
    <book-form @add:book="addBook" />
    <!-- BOOK -->
    <books-table v-on:delete-book="deleteBook" :booksSource="books" />
    <!-- BOOK -->

    <p></p>
    <h2 style="text-align: center; background-color: rgb(193, 240, 252)">
      <hr style="border-color: white" />
      Autorzy
      <hr style="border-color: white" />
    </h2>
    <p></p>
    <person-form @add:person="addPerson" />
    <!-- AUTHOR -->
    <persons-table v-on:delete-person="deletePerson" :personsSource="persons" />
    <!-- AUTHOR -->
    <!-- <persons-table v-on:update-person="updatePerson" :personsSource="persons" style="display:none;" />-->
    <!--  <persons-table v-on:update-person="updatePerson" :personsSource="persons" />-->
  </div>
</template>

<script>
import BooksTable from "@/components/BooksTable.vue"; //BOOK
import BookForm from "@/components/BookForm.vue"; //BOOK

import PersonsTable from "@/components/PersonsTable.vue"; //AUTHOR
import PersonForm from "@/components/PersonForm.vue"; //AUTHOR

export default {
  name: "App",
  components: {
    BooksTable, //BOOK
    BookForm, //BOOK

    PersonsTable, //AUTHOR
    PersonForm, //AUTHOR
  },
  data() {
    return {
      books: [], //BOOK

      persons: [], //AUTHOR
    };
  },

  methods: {
    /******************** BOOK  ********************/
    addBook(book) {
      this.books = [...this.books, book];
      const str = "http://127.0.0.1:8080/post/books";
      //postData(str, book);
      const requestOptions = {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          title: book.title,
          authorId: book.authorId,
          pages: book.pages,
        }),
      };
      fetch(str, requestOptions);

      this.getBooks();
    },
    async getBooks() {
      try {
        const response = await fetch("http://127.0.0.1:8080/get/books");
        const data = await response.json();
        this.books = data;
      } catch (error) {
        console.error(error);
      }
    },
    deleteBook(id) {
      // this.books = this.books.filter((book) => book.id !== id);
      const str = "http://127.0.0.1:8080/books/" + id;
      //postData(str, book);
      fetch(str, { method: "DELETE" });
      this.getBooks();
    },
    async postData(url = "", data = {}) {
      // Default options are marked with *
      const response = await fetch(url, {
        method: "POST", // *GET, POST, PUT, DELETE, etc.
        mode: "cors", // no-cors, *cors, same-origin
        cache: "no-cache", // *default, no-cache, reload, force-cache, only-if-cached
        credentials: "same-origin", // include, *same-origin, omit
        headers: {
          "Content-Type": "application/json",
          // 'Content-Type': 'application/x-www-form-urlencoded',
        },
        redirect: "follow", // manual, *follow, error
        referrerPolicy: "no-referrer", // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
        body: JSON.stringify(data), // body data type must match "Content-Type" header
      });
      return response.json(); // parses JSON response into native JavaScript objects
    },

    /******************** AUTHOR  ********************/

    addPerson(person) {
      //this.persons = [...this.persons, person];
      const requestOptions = {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          name: person.name,
          surname: person.surname,
        }),
      };
      fetch("http://localhost:8080/post/authors", requestOptions);
    },
    async getPersons() {
      try {
        const response = await fetch("http://127.0.0.1:8080/get/authors"); //jsonplaceholder.typicode.com/users')
        const data = await response.json();
        console.info(data);
        this.persons = data;
      } catch (error) {
        console.error(error);
      }
    },
    async deletePerson(id) {
      fetch("http://127.0.0.1:8080/authors/" + id, { method: "DELETE" });
      this.getPersons();
      /*  try {
        const response = await fetch("http://127.0.0.1:8080/get/authors"); //jsonplaceholder.typicode.com/users')
        const data = await response.json();
        this.persons = data;
      } catch (error) {
        console.error(error);
      }*/
      //jsonplaceholder.typicode.com/users')
      //const data1 = response.json()
    },
    async updatePerson(id, author_name, author_surname) {
      const requestOptions = {
        method: "PUT",
        body: JSON.stringify({ name: author_name, surname: author_surname }),
      };
      await fetch("http://127.0.0.1:8080/put/authors/" + id, requestOptions);
      this.getPersons();
    },
  },
  mounted() {
    this.getBooks(); //BOOK

    this.getPersons(); //AUTHOR
  },
};
</script>

<style>
button {
  background: #009435;
  border: 1px solid #009435;
}
.small-container {
  max-width: 680px;
}
</style>
