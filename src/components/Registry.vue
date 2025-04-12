<template>
    <v-container>
      <v-card class="mx-auto" max-width="400" style="margin-top: 100px;">
        <v-card-title class="headline justify-center">สมัครสมาชิก ไอ่เวร!!</v-card-title>
        <v-card-text>
          <v-text-field label="ชื่อ" v-model="formData.firstName" required></v-text-field>
          <v-text-field label="นามสกุล" v-model="formData.lastName" required></v-text-field>
          <v-text-field label="Username" v-model="formData.username" required></v-text-field>
          <v-text-field label="Password" v-model="formData.password" type="password" required></v-text-field>
          <v-select label="Role" :items="roles" v-model="formData.role" required></v-select>
        </v-card-text>
        <v-card-actions class="justify-center">
          <v-btn color="primary" @click="register">ลงทะเบียน</v-btn>
          <v-btn color="red" @click="cancel">ยกเลิก</v-btn>
        </v-card-actions>
      </v-card>
    </v-container>
  </template>
  
  <script>
  export default {
    data() {
      return {
        formData: {
          firstName: '',
          lastName: '',
          username: '',
          password: '',
          role: ''
        },
        roles: ['ผู้ใช้ทั่วไป','ร้านค้า']
      };
    },
    methods: {
      register() {
        // โค้ดสำหรับส่งข้อมูลไปยังเซิร์ฟเวอร์
        console.log(this.formData);
        // ...
      },
      cancel() {
        // โค้ดสำหรับยกเลิกการลงทะเบียน
        console.log('ยกเลิกการลงทะเบียน');
        // ...
      }
    }
  };
  </script>