<!-- ExampleComponent.vue -->

<template>
  <div>
  <div :class="{ error: v$.firstName.$errors.length }">
    <input v-model="firstName">
    <div class="input-errors" v-for="error of v$.firstName.$errors" :key="error.$uid">
      <div class="error-msg">{{ error.$message }}</div>
    </div>
  </div>
  <div :class="{ error: v$.lastName.$errors.length }">
    <input v-model="lastName">
    <div class="input-errors" v-for="error of v$.lastName.$errors" :key="error.$uid">
      <div class="error-msg">{{ error.$message }}</div>
    </div>
  </div>
  <div :class="{ error: v$.contact.email.$errors.length }">
    <input v-model="contact.email">
    <div class="input-errors" v-for="error of v$.contact.email.$errors" :key="error.$uid">
      <div class="error-msg">{{ error.$message }}</div>
    </div>
  </div>
    <button @click="submitForm">Submit</button>
  </div>
</template>

<script>
import { useVuelidate } from '@vuelidate/core'
import { required,minLength, email,helpers } from '@vuelidate/validators'


export default {
  setup () {
    return { v$: useVuelidate() }
  },
  data () {
    return {
      firstName: '',
      lastName: '',
      contact: {
        email: ''
      }
    }
  },
  validations () {
    return {
      firstName: {
        required : helpers.withMessage("First Name Is Required",required),
        minLength: helpers.withMessage("First Name Is Required Atleast 3 character ",minLength(3))
      }, // Matches this.firstName
      lastName: { 
        required : helpers.withMessage("Last name Is Required",required),
        
       }, // Matches this.lastName
      contact: {
        email: { 
        required : helpers.withMessage("Email Is Required",required),
        email: helpers.withMessage("Please enter valid email",email)
         } // Matches this.contact.email
      }
    }
  },
  methods: {
    async submitForm() {
      const isValid = await this.v$.$validate();
      if (isValid) {
        // Form is valid, submit it
        console.log('Form submitted');
      } else {
        // Form has validation errors, handle them
        console.log('Form has validation errors');
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
body {background: #fff;}
</style>
