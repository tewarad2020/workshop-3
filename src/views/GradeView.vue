<template>
  <div style="display: flex; flex-direction: column; height: 100vh; background-color: #B0BEC5;">
    <div style="flex: 1; display: flex; flex-direction: column; align-items: center; justify-content: center; margin-bottom: 20px;">
      <h1>โปรแกรมตัดเกรด</h1>
      <div style="display: flex; align-items: center; margin-bottom: 20px;">
        <label for="score" style="margin-right: 10px; margin-top: 50px">คะแนน:</label>
        <input type="text" id="score" v-model="score" style="text-align: right; border: 1px solid #ccc; padding: 5px; margin-top: 50px">
        <v-btn @click="calculateGrade" style="background-color: #9575CD; color: white; padding: 5px 10px; border: none; margin-left: 10px; margin-top: 50px;  ">คำนวณเกรด</v-btn>
      </div>
      <p v-if="grade" :style="{ fontSize: '20px', color: grade === 'F' ? 'red' : 'green' }">เกรดที่ได้: {{ grade }}</p>
      <p v-if="errorMessage" style="color: red;">{{ errorMessage }}</p>
    </div>
    <div style="width: 100%; text-align: center; padding: 3px 0; background-color: #37474F;">
      <p style="font-size: 13px; color: #FFFFFF; margin: 0;">Copyright © by Khomson 2025</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      score: '',
      grade: '',
      errorMessage: ''
    };
  },
  methods: {
    calculateGrade() {
      // ล้างค่าเก่า
      this.grade = '';
      this.errorMessage = '';

      // ตรวจสอบข้อมูลนำเข้า
      const score = parseFloat(this.score);
      if (isNaN(score) || score < 0 || score > 100) {
        this.errorMessage = 'กรุณาป้อนคะแนนเป็นตัวเลขระหว่าง 0 ถึง 100';
        return;
      }

      // คำนวณเกรด
      if (score >= 80) {
        this.grade = 'A';
      } else if (score >= 70) {
        this.grade = 'B';
      } else if (score >= 60) {
        this.grade = 'C';
      } else if (score >= 50) {
        this.grade = 'D';
      } else {
        this.grade = 'F';
      }
    }
  }
};
</script>