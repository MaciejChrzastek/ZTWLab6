<template>
    <div id="person-form">
        <form @submit.prevent="handleSubmit">
       <!-- <label>Id</label>
        <input
        v-model="person.id"
        type="number"
        :class="{ 'has-error': submitting }" 
        @focus="clearStatus"
        @keypress="clearStatus"
        />-->
        <label>Imię</label>
        <input
        v-model="person.name"
        type="text"
        :class="{ 'has-error': submitting && invalidName }" 
        @focus="clearStatus"
        @keypress="clearStatus"
        />
        <label>Nazwisko</label>
        <input
        v-model="person.surname"
        type="text"
        :class="{ 'has-error': submitting && invalidSurname }" 
        @focus="clearStatus"
        @keypress="clearStatus"
        />
        <!--
        <label>Email</label>
        <input
        v-model="person.email"
        type="text"
        :class="{ 'has-error': submitting && invalidEmail }"
        @focus="clearStatus"
        />
        <label>Telefon</label>
        <input
        v-model="person.phone"
        type="text"
        :class="{ 'has-error': submitting && invalidPhone }"
        @focus="clearStatus"
        @keypress="clearStatus"
        />
        -->
        <p v-if="error && submitting" class="error-message">
        Proszę wypełnić wskazane pola formularza
        </p>
        <p v-if="success" class="success-message">
        Dane poprawnie zapisano
        </p>
        <button>Dodaj autora</button>
        </form>
    </div>
</template>
<script>
    export default {
        name: 'person-form',
        data() {
            return {
                submitting: false,
                error: false,
                success: false, 
                person: {
                    id: 0,
                    name: '',
                    surname: ''
                    //email: '',
                    //phone: '',
                },
            }
        },methods: {
            handleSubmit() {
                this.submitting = true
                this.clearStatus()
                //check form fields
                if (this.invalidName || this.invalidSurname ){ //|| this.invalidEmail || this.invalidPhone) {
                    this.error = true
                    return
                } 

                this.$emit('add:person', this.person) 

                //clear form fields
                this.person = {
                    id:0,
                    name: '',
                    surname: ''
               // email: '',
               // phone: '',
                }
                this.error = false
                this.success = true
                this.submitting = false
                },

                clearStatus() {
                this.success = false
                this.error = false
                }, 

            
        }, computed: {
                    invalidName() {
                        return this.person.name === ''
                    },
                    invalidSurname() {
                        return this.person.surname === ''
                    }, 
                    /*
                    invalidEmail() {
                        return this.person.email === ''
                    },
                    invalidPhone() {
                        return this.person.phone === ''
                    },*/
                }, 

    }
</script>
<style scoped>
    form {
        margin-bottom: 2rem;
    }


    [class*='-message'] {
    font-weight: 500;
    }
    .error-message {
    color: #d33c40;
    }
    .success-message {
    color: #32a95d;
    } 

</style>
