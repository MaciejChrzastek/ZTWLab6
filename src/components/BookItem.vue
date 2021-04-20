<template>
  <div class="container">
    <div class="book-item">
      <p>
        {{ book.title }} <br />
        {{ book.authorId }}<br />
        {{ book.pages }}<br />
        <button @click="$emit('delete-book', book.id)" class="del">
          DELETE
        </button>
        <button @click="update" class="update">UPDATE</button>
      </p>
    </div>

    <div v-if="this.flag">
      <form @submit.prevent="handleSubmit">
        <label>Tytuł</label>
        <input
          v-model="boook.title"
          type="text"
          :class="{ 'has-error': submitting && invalidTitle }"
          @focus="clearStatus"
          @keypress="clearStatus"
        />
        <label>Autor ID</label>
        <input
          v-model="book.authorId"
          type="text"
          :class="{ 'has-error': submitting && invalidAuthorId }"
          @focus="clearStatus"
        />
        <label>Strony</label>
        <input
          v-model="book.pages"
          type="text"
          :class="{ 'has-error': submitting && invalidPages }"
          @focus="clearStatus"
          @keypress="clearStatus"
        />
        <p v-if="error && submitting" class="error-message">
          Proszę wypełnić wskazane pola formularza
        </p>
        <p v-if="success" class="success-message">Dane poprawnie zapisano</p>
        <button>Aktualizuj książkę</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "BookItem",
  props: ["book", "booksSource"],
  methods: {
    update() {
      this.flag = !this.flag;
    },
    handleSubmit() {
      this.submitting = true;
      this.clearStatus();
      //check form fields
      if (this.invalidTitle || this.invalidAuthorId || this.invalidPages) {
        this.error = true;
        return;
      }

      this.booksSource.filter((book) => {
        if (book.id == this.book.id) {
          book.title = this.updatedbook.title;
          book.authorId = this.updatedbook.authorId;
          book.pages = this.updatedbook.pages;
          const str = "http://127.0.0.1:8080/put/books/" + book.id;

          const requestOptions = {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({
              book,
            }),
          };
          fetch(str, requestOptions);
          //postData(str,book)
        }
      });

      this.error = false;
      this.success = true;
      this.submitting = false;
      this.flag = false;
    },

    clearStatus() {
      this.success = false;
      this.error = false;
    },
    async postData(url, data) {
      //url = '', data = {}) {
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
  },
  data() {
    return {
      flag: false,
      submitting: false,
      error: false,
      success: false,
      updatedbook: {
        title: "",
        authorId: "",
        pages: "",
      },
    };
  },
  computed: {
    invalidTitle() {
      return this.updatedbook.title === "";
    },
    invalidAuthorId() {
      return this.updatedbook.authorId === "";
    },
    invalidPages() {
      return this.updatedbook.pages === "";
    },
  },
};
</script>

<style scoped>
.person-item {
  background: #f4f4f4;
  padding: 10px;
  margin: 20px;
  border-bottom: 1px #ccc dotted;
}

.is-complete {
  text-decoration: line-through;
}

.del {
  background: #ff0000;
  color: #fff;
  border: none;
  padding: 5px 9px;
  border-radius: 50%;
  cursor: pointer;
  float: right;
}
.update {
  background: darkgreen;
  color: #fff;
  border: none;
  padding: 5px 9px;
  border-radius: 50%;
  cursor: pointer;
  float: right;
}
</style>


