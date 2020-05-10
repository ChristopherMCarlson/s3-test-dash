// src/components/SignUp.vue
<template>
  <div class="px-5">
    <h2 class="mx-auto">{{ formState === 'signUp' ? 'Sign Up' : 'Confirm Sign Up' }}</h2>
      <v-form @submit='signUp' v-if="formState === 'signUp'">
        <v-text-field v-model='form.username' />
        <v-text-field type='password' v-model='form.password' />
        <v-text-field v-model='form.email' />
      </v-form>
      <v-form @submit='confirmSignUp' v-if="formState === 'confirmSignUp'">
        <v-text-field v-model='form.authCode' />
        <button v-on:click='confirmSignUp' class='button'>Confirm Sign Up</button>
      </v-form>
    <v-card-actions>
      <v-btn v-on:click='signUp' class='button' v-if="formState === 'signUp'">Sign Up</v-btn>
      <v-btn v-on:click='confirmSignUp' class='button' v-if="formState === 'confirmSignUp'">Confirm Sign Up</v-btn>
    </v-card-actions>
  </div>
</template>

<script>
import { Auth } from 'aws-amplify'

export default {
  name: 'home',
  props: ['toggle'],
  data() {
    return {
      formState: 'signUp',
      form: {
        username: '',
        password: '',
        email: ''
      }
    }
  },
  methods: {
    async signUp() {
      const { username, password, email } = this.form
      await Auth.signUp({
        username, password, attributes: { email }
      })
      this.formState = 'confirmSignUp'
    },
    async confirmSignUp() {
      const { username, authCode } = this.form
      await Auth.confirmSignUp(username, authCode)
      alert('successfully signed up! Sign in to view the app.')
      this.toggle()
    }
  }
}
</script>