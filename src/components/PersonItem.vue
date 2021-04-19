<template>

<div class="container">
  <div class="person-item" >
    <p>
        {{ person.name }} <br>
        {{ person.email }}<br>
        {{ person.phone }}<br>
      <button @click="$emit('delete-person',person.id)" class="del">DELETE</button>
      <button @click="update" class="update">UPDATE</button>

    </p>
  </div>

  <div v-if="this.flag">
      <form @submit.prevent="handleSubmit">
        <label>Imię i nazwisko</label>
        <input
        v-model="updatedperson.name"
        type="text"
        :class="{ 'has-error': submitting && invalidName }" 
        @focus="clearStatus"
        @keypress="clearStatus"
        />
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
        />
        <p v-if="error && submitting" class="error-message">
        Proszę wypełnić wskazane pola formularza
        </p>
        <p v-if="success" class="success-message">
        Dane poprawnie zapisano
        </p>
        <button>Aktualizuj kontakt</button>
        </form>
  </div>
  </div>
    
</template>

<script>

export default {
    name:"PersonItem",
    props:['person','personsSource'],
    methods:{
        update(){
            this.flag = !this.flag
                
        },handleSubmit() {
                this.submitting = true
                this.clearStatus()
                //check form fields
                if (this.invalidName || this.invalidEmail || this.invalidPhone) {
                    this.error = true
                    return
                } 
                
                this.personsSource.filter((person) => {
                    if(person.id == this.person.id){
                        person.name = this.updatedperson.name
                        person.phone = this.updatedperson.phone
                        person.email = this.updatedperson.email
                    }
                })

                
                this.error = false
                this.success = true
                this.submitting = false
                this.flag = false
                },

                clearStatus() {
                this.success = false
                this.error = false
                }, 
    },
    data(){
        return{
            flag:false,
            submitting: false,
                error: false,
                success: false, 
                updatedperson: {
                    name: '',
                    email: '',
                    phone: '',
                },
                
        }
    }, computed: {
                    invalidName() {
                        return this.updatedperson.name === ''
                    },
                    invalidEmail() {
                        return this.updatedperson.email === ''
                    },
                    invalidPhone() {
                        return this.updatedperson.phone === ''
                    },
                }, 
}
    
</script>

<style scoped>
.person-item {
    background: #f4f4f4;
    padding: 10px;
    margin:20px;
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
  .update{
    background: darkgreen;
    color: #fff;
    border: none;
    padding: 5px 9px;
    border-radius: 50%;
    cursor: pointer;
    float: right;
  }

</style>


