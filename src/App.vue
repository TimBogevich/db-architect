<template>
  <v-app>
    <v-app-bar
      dense
      app
      clipped-left
      color="primary"
      dark
    >
         <router-link to="/">
          <v-toolbar-title class="logo_text">
              <span>DB Tools</span>
          </v-toolbar-title>
         </router-link>
      <v-spacer></v-spacer>
      <v-btn v-if="!user"  text @click="loginDialog()">LOGIN</v-btn>
      <userMenu  v-if="user"/>

      <v-progress-linear
      :active="loaderIsVisible()"
      indeterminate
      bottom
      fixed
      color="white"
      height="6"
      ></v-progress-linear>

    </v-app-bar>

    <v-navigation-drawer
      app
      clipped
    >

      <v-list v-for="item in navBarLinks" :key="item.link">
        <v-list-item link :to="item.link">
          <v-list-item-content>
            <v-list-item-title>
              <v-icon>
                {{item.icon}}
              </v-icon>
              {{item.name}}
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
import userMenu from './components/userMenu';
import {get,sync} from 'vuex-pathify'
import firebase from 'firebase'

export default {
  name: 'App',

  components: {
    userMenu,
  },

  data: () => ({
    navBarLinks : [
      {name : "Table editor", link : "table_editor", icon : "mdi-table"},
      {name : "Storage Calculator", link : "storage_calc", icon : "mdi-store"},
    ]
  }),

  computed: {
    user: sync("general/user"),
    loaderIsVisible() {
      return this.$store.getters["vuexActionTracker/hasRunningActions"]
    },
  },

  methods: {
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


<style >
  @font-face {
    font-family: "ethnocentric";
    src: local("ethnocentric"),
    url(./assets/ethnocentric_rg.ttf) format("truetype");
  }
 
  .logo_text {
     font-family: "ethnocentric";
     color: white;
  }
</style>