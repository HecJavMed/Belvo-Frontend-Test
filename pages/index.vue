<template>
  <div>
    <v-container v-if="!loggedIn">
      <AuthPage @loggedIn="login" />
    </v-container>
    <v-container v-else>
      <HomePage @logout="logout" />
    </v-container>
  </div>
</template>

<script>
// import AuthPage from '../components/AuthPage.vue'

export default {
  data () {
    return {
      loggedIn: false,
      userEmail: ''
    }
  },
  methods: {
    login (email) {
      this.loggedIn = true
      this.userEmail = email
    },
    logout () {
      this.loggedIn = false
      this.userEmail = ''
    }
  }
}
</script>

<style scoped>
/* Add your styles here */
</style>
