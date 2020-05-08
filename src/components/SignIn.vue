// src/components/SignIn.vue
<template>
  <div>
    <h2>Sign In</h2>
    <v-form @submit="signIn">
      <v-text-field v-model='form.username' />
      <v-text-field type='password' v-model='form.password' />
      <button v-on:click='signIn' class='button'>Sign In</button>
    </v-form>
  </div>
</template>

<script>
import { Auth } from 'aws-amplify'
import { AmplifyEventBus } from 'aws-amplify-vue'
export default {
  name: 'home',
  data() {
    return {
      form: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    async signIn() {
      const { username, password } = this.form
      await Auth.signIn(username, password)
      AmplifyEventBus.$emit('authState', 'signedIn')
      this.$router.push('/profile')
    }
  }
}
</script>