<template>
  <v-app>
      <v-app-bar
      color="deep-purple"
      dark
      dense
      app
    >
      <v-app-bar-nav-icon @click="drawer = true"></v-app-bar-nav-icon>

      <v-toolbar-title>My Dash</v-toolbar-title>
      <v-spacer></v-spacer>

    </v-app-bar>

    <v-navigation-drawer
      v-model="drawer"
      absolute
      temporary
    >
      <v-list
        nav
        dense
      >
        <v-list-item-group
          active-class="deep-purple--text text--accent-4"
        >
        <router-link tag="p" to="/auth" v-if="!signedIn">
          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-login</v-icon>
            </v-list-item-icon>
            <v-list-item-title>Login</v-list-item-title>
          </v-list-item>
        </router-link>
        <router-link tag="p" to="/">
          <v-list-item>
            <v-list-item-icon>
                  <v-icon>mdi-home</v-icon>
                </v-list-item-icon>
                <v-list-item-title>Home</v-list-item-title>
              </v-list-item>
            </router-link>
            <router-link tag="p" to="/profile">
              <v-list-item>
                <v-list-item-icon>
                  <v-icon>mdi-account</v-icon>
                </v-list-item-icon>
                <v-list-item-title>Account</v-list-item-title>
              </v-list-item>
            </router-link>
            <router-link tag="p" to="/newPost">
              <v-list-item>
                <v-list-item-icon>
                  <v-icon>mdi-pencil</v-icon>
                </v-list-item-icon>
                <v-list-item-title>Write Post</v-list-item-title>
              </v-list-item>
            </router-link>
              <v-list-item click="signOut" v-if="signedIn">
                <v-list-item-icon>
                  <v-icon>mdi-logout</v-icon>
                </v-list-item-icon>
                <v-list-item-title>Logout</v-list-item-title>
              </v-list-item>

        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>
    <v-content>
      <router-view/>
    </v-content>
  </v-app>
</template>

<script>
    import { AmplifyEventBus } from 'aws-amplify-vue'
    import { Auth } from 'aws-amplify'
    export default {
      name: 'app',
      data() {
        return {
          signedIn: false,
          drawer: false,
        }
      },
      beforeCreate() {
        AmplifyEventBus.$on('authState', info => {
          if (info === 'signedIn') {
            this.signedIn = true
            this.$router.push('/profile')
          }
          if (info === 'signedOut') {
            this.$router.push('/auth')
            this.signedIn = false
          }
        });
        Auth.currentAuthenticatedUser()
          .then(user => {
            this.signedIn = true
          })
          .catch(() => this.signedIn = false)
      },
      methods: {
        async signOut() {
          try {
              await Auth.signOut();
          } catch (error) {
              console.log('error signing out: ', error);
          }
        }
      }
    }
    </script>