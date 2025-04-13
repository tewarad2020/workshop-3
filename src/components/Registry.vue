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
          role: 'user'
        },
        roles: ['user']
      };
    },
    methods: {
      register() {
        this.axios.post(`${process.env.VUE_APP_API_URL}/register`, this.formData)
          .then(res => {
            alert('ลงทะเบียนสำเร็จ');
            this.$router.push('/login');
          })
          .catch(error => {
            alert(`ลงทะเบียนล้มเหลว, ข้อความผิดพลาด: ${error.response.data.message}`);
          });
      },
      cancel() {
        this.$router.push('/login');
      }
    }
  };
  </script>