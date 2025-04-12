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
          <v-btn color="red" @click="login">Login</v-btn>
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
    methods: {
      login() {
        // โค้ดสำหรับส่งข้อมูลไปยังเซิร์ฟเวอร์
        console.log({ username: this.username, password: this.password });
        // ...
      },
      goBack() {
        // โค้ดสำหรับกลับหน้าหลัก
        console.log('กลับหน้าหลัก');
        // ...
      },
      goToFanPage() {
        // โค้ดสำหรับไปยังแฟนเพจ
        console.log('แฟนเพจ');
        // ...
      }
    }
  };
  </script>