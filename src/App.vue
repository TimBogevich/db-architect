<template>
  <v-app>
    <v-app-bar
      app
      clipped-left
      color="primary"
      dark
    >
      <v-spacer></v-spacer>
      <v-btn v-if="!user" color="success" @click="loginDialog()">LOGIN</v-btn>
      <v-avatar v-else
         @click="logOut()"
        color="primary"
      >
        <img
          :src="user.photoURL"
          :alt="user.name"
        >
      </v-avatar>

    </v-app-bar>

    <v-navigation-drawer
      app
      clipped
    >

      <v-list dense>
        <v-list-item link to="table_editor">
          <v-list-item-content>
            <v-list-item-title>
              <v-icon>
                mdi-table
              </v-icon>
              Table editor
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
      
    </v-navigation-drawer>

    <v-main>
      <router-view></router-view>
    </v-main>

    <dialogs-wrapper name="areYouSure"></dialogs-wrapper>
    <dialogs-wrapper name="login"></dialogs-wrapper>

  </v-app>
</template>

<script>
import HomePage from './components/HomePage';
import {get,sync} from 'vuex-pathify'
import firebase from 'firebase'

export default {
  name: 'App',

  components: {
    HomePage,
  },

  data: () => ({
  }),

  computed: {
    user: sync("general/user"),
  },

  methods: {
    logOut() {
      this.$store.dispatch("general/logOut")
    },
    login(){
      let store = this.$store
      firebase.auth().onAuthStateChanged(async (user) => {
        if (user) {
          store.dispatch("general/retrieveUser", user.uid)
        } else {
          let logout = await firebase.auth().signOut()
          this.user = null
        }
      });
    },
    async loginDialog() {
      let response = await this.$dialogLogin()
    }
  },
    mounted() {
      this.login()
    },
};
</script>
