<template>
  <div class="container">
    <div class="person-item">
      <p>
        {{ person.id }} <br />
        {{ person.name }} <br />
        {{ person.surname }} <br />
        <!--{{ person.email }}<br>
        {{ person.phone }}<br> -->
        <button @click="$emit('delete-person', person.id)" class="del">
          DELETE
        </button>
        <button @click="update" class="update">UPDATE</button>
        <!--@click="update"-->
      </p>
    </div>

    <div id="person-update-form" v-if="this.flag">
      <form @submit.prevent="handleSubmit">
        <!--<label>Id</label>
        <p v-bind:value="person.id">person.id</p> -->
        <p>
          <label>Imię</label>
          <input
            v-model="updatedperson.name"
            type="text"
            :class="{ 'has-error': submitting && invalidName }"
            @focus="clearStatus"
            @keypress="clearStatus"
          />
          <label>Nazwisko</label>
          <input
            v-model="updatedperson.surname"
            type="text"
            :class="{ 'has-error': submitting && invalidSurname }"
            @focus="clearStatus"
            @keypress="clearStatus"
          />
          <!--
        <label>Email</label>
        <input
        v-model="updatedperson.email"
        type="text"
        :class="{ 'has-error': submitting && invalidEmail }"
        @focus="clearStatus"
        />
        <label>Telefon</label>
        <input
        v-model="updatedperson.phone"
        type="text"
        :class="{ 'has-error': submitting && invalidPhone }"
        @focus="clearStatus"
        @keypress="clearStatus"
        /> -->
        </p>

        <p v-if="error && submitting" class="error-message">
          Proszę wypełnić wskazane pola formularza
        </p>
        <p v-if="success" class="success-message">Dane poprawnie zapisano</p>
        <button>Aktualizuj kontakt</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "PersonItem",
  props: ["person", "personsSource"],
  methods: {
    async update() {
      this.flag = !this.flag;
    },
    async handleSubmit() {
      this.submitting = true;
      this.clearStatus();
      //check form fields
      if (this.invalidName || this.invalidSurname) {
        //} || this.invalidEmail || this.invalidPhone) {
        this.error = true;
        return;
      }

      //    this.$emit('update-person', this.person.id, this.updatedperson.name, this.updatedperson.surname )


const str = "http://localhost:8080/put/authors/" + this.person.id;

          const requestOptions = {
            method: "PUT",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({
              name: this.updatedperson.name,
              surname: this.updatedperson.surname,
            }),
          };
          fetch(str, requestOptions);
      this.personsSource.filter((person) => {
                    if(person.id == this.person.id){
                        person.name = this.updatedperson.name
                        person.surname = this.updatedperson.surname
                        //person.phone = this.updatedperson.phone
                        //person.email = this.updatedperson.email
                    }
      })    
                
          
          //postData(str,book)
        
      

/*
      const requestOptions = {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          name: this.updatedperson.name,
          surname: this.updatedperson.surname,
        }),
      };
      fetch(
        "http://localhost:8080/put/authors/" + this.person.id,
        requestOptions
      );
      //this.$emit('reread')
      
      try {
        const response = await fetch("http://127.0.0.1:8080/get/authors"); //jsonplaceholder.typicode.com/users')
        const data = await response.json();
        console.info(data);
        this.persons = data;
      } catch (error) {
        console.error(error);
      }
*/


      /*
                this.personsSource.filter((person) => {
                    if(person.id == this.person.id){
                        person.name = this.updatedperson.name
                        person.surname = this.updatedperson.surname
                        //person.phone = this.updatedperson.phone
                        //person.email = this.updatedperson.email
                    }
                })*/

      this.error = false;
      this.success = true;
      this.submitting = false;
      this.flag = false;
    },

    clearStatus() {
      this.success = false;
      this.error = false;
    },
  },
  data() {
    return {
      flag: false,
      submitting: false,
      error: false,
      success: false,
      updatedperson: {
        id: 0,
        name: "",
        surname: "",
        // email: '',
        // phone: '',
      },
    };
  },
  computed: {
    invalidName() {
      return this.updatedperson.name === "";
    },
    invalidSurname() {
      return this.updatedperson.surname === "";
    },
    /*
                    invalidEmail() {
                        return this.updatedperson.email === ''
                    },
                    invalidPhone() {
                        return this.updatedperson.phone === ''
                    },*/
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


