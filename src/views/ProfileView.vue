<template>
  <div style="display: flex; flex-direction: column; height: auto; background-color: #BDBDBD">
    <div style="flex: 1; display: flex; flex-direction: column; align-items: center; justify-content: center;">
      <h1>Hello World!!</h1>
      <div style="margin-top: 30px;">
        <v-responsive
          class="circle-wrapper"
          max-width="300"
          max-height="300"
        >
          <Profile />
        </v-responsive>
      </div>
      <div>
        <p style="margin-top: 40px; font-size: 25px; color: #616161; text-align: center;"><b>
          Khomson  Suangkaew  (Big)</b></p>
        <p style="margin-top: 20px;">ผมชื่อนายคมสันต์  ทรวงแก้ว ชื่อเล่น บิ๊ก จบจากมหาวิทยาลัยเชียงใหม่ สาขาวิศวกรรมคอมพิวเตอร์</p>
        <p style="margin-top: 20px;">งานอดิเรก คือ ดีเจ สิ่งที่ชอบ คือ ชอบศึกษาความรู้เกี่ยวกับการลงทุน และเทคโนโลยีใหม่ ๆ อยู่เสมอ</p>
        <p style="margin-top: 20px;">กีฬาที่ชอบ แบดมินตัน วอลเลย์บอล บาสเกตบอล ฟุตบอล ทีมฟุตบอลที่ชอบ คือ ทีมลิเวอร์พูล</p>
      </div>
      
      <div style="margin-bottom: 50px; margin-top: 30px;">
        <Graduate />
    </div>
      
    </div>
    <div style="width: 100%; text-align: center; padding: 3px 0; background-color: #f0f0f0;">
      <p style="font-size: 13px; color: #888; margin: 0;">Copyright © by Khomson 2025</p>
    </div>
  </div>
</template>

<script>
import Profile from '@/components/Profile.vue';
import Graduate from '@/components/Graduate.vue';
export default {
  components: {
    Profile,
    Graduate
  }
}
</script>

<style scoped>
.circle-wrapper {
  border-radius: 50%;
  overflow: hidden;
  /* box-shadow: 0px 100px 100px rgba(0,0,0,0.3); */
  box-shadow: 10px 10px 10px rgba(0, 0, 0, 0.2);
}
</style>