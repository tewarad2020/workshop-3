<template>
    <div style="display: flex; flex-direction: column; align-items: center;">
      <v-img src="../assets/login.png" width="100" style="align-self: center; margin-bottom: 20px;"></v-img>
      <v-card class="mx-auto" max-width="400">
        <v-card-title class="headline justify-center">Login</v-card-title>
        <v-card-text>
          <v-text-field label="Email" v-model="username" required></v-text-field>
          <v-text-field label="Password" v-model="password" type="password" required></v-text-field>
        </v-card-text>
        <v-card-actions class="justify-center">
          <v-btn color="blue" @click="login">Login</v-btn>
          <v-btn color="green" @click="goToRegister">Register</v-btn>
        </v-card-actions>
      </v-card>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        username: '',
        password: ''
      };
    },
    created() {
      const token = this.$cookies.get('token');
      if (token) {
        this.$router.push('/shop');
      }
    },
    methods: {
      login() {
        this.axios.post(`${process.env.VUE_APP_API_URL}/login`, {
          username: this.username,
          password: this.password
        })
        .then(res => {
          const token = res.data.data.token
          this.$cookies.set('token', token, '3h')
          this.$router.push('/shop')
        })
        .catch(error => {
          alert(`Login failed, error message: ${error.response.data.message}`);
        });
      },
      goToRegister() {
        this.$router.push('/registry');
      },
    }
  };
  </script>