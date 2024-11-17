<template>
  <v-container fluid fill-height>
    <v-layout align-center justify-center>
      <v-flex xs12 sm8 md4>
        <v-card class="elevation-12">
          <v-toolbar dark color="primary">
            <v-toolbar-title>{{ isRegister ? stateObj.register.name : stateObj.login.name }} form</v-toolbar-title>
          </v-toolbar>
          <v-card-text>
            <form ref="form" @submit.prevent="isRegister ? register() : login()">
              <v-text-field
                v-model="email"
                name="email"
                label="Email"
                type="text"
                placeholder="email"
                required
              />

              <v-text-field
                v-model="password"
                name="password"
                label="Password"
                type="password"
                placeholder="password"
                required
              />

              <v-text-field
                v-if="isRegister"
                v-model="confirmPassword"
                name="confirmPassword"
                label="Confirm Password"
                type="password"
                placeholder="confirm password"
                required
              />
              <div class="red--text">
                {{ errorMessage }}
              </div>
              <v-btn type="submit" class="mt-4" color="primary" value="log in">
                {{ isRegister ? stateObj.register.name : stateObj.login.name }}
              </v-btn>
              <v-btn variant="text" class="grey--text mt-4" @click="isRegister = !isRegister;">
                {{ toggleMessage }}
              </v-btn>
            </form>
          </v-card-text>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  name: 'AuthPage',
  data () {
    return {
      email: '',
      password: '',
      confirmPassword: '',
      isRegister: false,
      errorMessage: '',
      stateObj: {
        register: {
          name: 'Register',
          message: 'Aleady have an Acoount? login.'
        },
        login: {
          name: 'Login',
          message: 'Register'
        }
      }
    }
  },
  computed: {
    toggleMessage: function () {
      return this.isRegister ? this.stateObj.register.message : this.stateObj.login.message
    }
  },
  methods: {
    async login () {
      try {
        const loginTmp = await this.$axios.$post('/api/login', { email: this.email, password: this.password })
        if (loginTmp.status === 'success') {
          this.$emit('loggedIn', this.email)
        }
      } catch (error) {
        this.errorMessage = 'Invalid email or password'
      }
    },
    async register () {
      if (this.password === this.confirmPassword) {
        try {
          const registerTmp = await this.$axios.$post('/api/register', { email: this.email, password: this.password })
          if (registerTmp.id !== undefined) {
            this.errorMessage = ''
            this.isRegister = false
            this.$refs.form.reset()
          }
        } catch (error) {
          this.errorMessage = 'Email already exists'
        }
      } else {
        this.errorMessage = 'password did not match'
      }
    }
  }
}
</script>

<style scoped>
.login {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.login h1 {
  text-align: center;
}

.login div {
  margin-bottom: 15px;
}

.login label {
  display: block;
  margin-bottom: 5px;
}

.login input {
  width: 100%;
  padding: 8px;
  box-sizing: border-box;
}

.login button {
  width: 100%;
  padding: 10px;
  background-color: #007BFF;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.login button:hover {
  background-color: #0056b3;
}
</style>
