<template>
    <div id="book-form">
        <form @submit.prevent="handleSubmit">
        <label>Tytuł</label>
        <input
        v-model="book.title"
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
        <p v-if="success" class="success-message">
        Dane poprawnie zapisano
        </p>
        <button>Dodaj książkę</button>
        </form>
    </div>
</template>
<script>
    export default {
        name: 'book-form',
        data() {
            return {
                submitting: false,
                error: false,
                success: false, 
                book: {
                    title: '',
                    authorId: '',
                    pages: '',
                },
            }
        },methods: {
            handleSubmit() {
                this.submitting = true
                this.clearStatus()
                //check form fields
                if (this.invalidTitle || this.invalidAuthorId || this.invalidPages) {
                    this.error = true
                    return
                } 

                this.$emit('add:book', this.book) 

                //clear form fields
                this.book = {
                title: '',
                authorId: '',
                pages: '',
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
                    invalidTitle() {
                        return this.book.title === ''
                    },
                    invalidAuthorId() {
                        return this.book.authorId === ''
                    },
                    invalidPages() {
                        return this.book.pages === ''
                    },
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
