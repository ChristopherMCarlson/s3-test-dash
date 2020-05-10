// src/components/Auth.vue
<template>
    <v-card class="mx-auto my-5" max-width="344" outlined>
      <sign-up :toggle='toggle' v-if="formState === 'signUp'"></sign-up>
      <sign-in v-if="formState === 'signIn'"></sign-in>
      <p v-on:click="toggle" class="toggle mx-5" style="cursor: pointer">{{ formState === 'signUp' ? 'Already have an account? Sign In' : 'Need an account? Sign Up'}}</p>
    </v-card>
</template>

<script>
import { AmplifyEventBus } from 'aws-amplify-vue'
import SignUp from './SignUp'
import SignIn from './SignIn'

export default {
  name: 'app',
  data() {
    return {
      formState: 'signUp'
    }
  },
  methods: {
    toggle() {
      this.formState === 'signUp' ? this.formState = 'signIn' : this.formState = 'signUp'
    }
  },
  components: {
    SignUp,
    SignIn
  }
}
</script>