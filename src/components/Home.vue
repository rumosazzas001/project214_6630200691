<template>
  <div class="home">
    <h1 class="fade-in">Welcome to My Portfolio</h1>
    <p> </p>
    <transition name="slide-in">
      <div class="frame">
        <div class="image-section">
          <img src="@/assets/img/HomePic.png" alt="Description of image" class="portfolio-image fade-in" />
          <p
            class="image-description"
            v-for="(desc, index) in descriptions"
            :key="index"
            :style="{ animationDelay: `${index * 0.2}s` }"
          >
            {{ desc }}
          </p>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
      descriptions: [
        "เว็บไซต์นี้ได้รับการพัฒนาขึ้นเพื่อใช้เป็นสื่อในการนำเสนอข้อมูลส่วนตัว",
        "และผลงานที่เกี่ยวข้องกับรายวิชา 01418214 65 การฝึกปฏิบัติการพัฒนาซอฟต์แวร์",
        "โดยนำเสนอให้กับ อาจารย์โรจฤทธิ์ สว่างแจ้ง",
        "", // บรรทัดว่าง
        "",
        "",
        ""
      ]
    };
  }
};
</script>

<style scoped>

.home {
  padding: 20px;
  color: #ffffff;
  font-family: 'Sarabun', sans-serif; /* ฟอนต์หลักสำหรับเนื้อหา */
}

h1 {
  font-family: 'Prompt', sans-serif; /* ฟอนต์สำหรับหัวข้อ */
  font-weight: 700;
}

.frame {
  max-width: 1200px;
  width: 110%; 
  background-color: #ffffff;
  margin: 20px auto;
  padding: 20px;
  border-radius: 20px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  transition: background-color 0.3s ease;
}

.image-section {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.portfolio-image {
  width: 400px;
  max-width: 600px;
  height: auto;
  margin: 10px auto;
}

.image-description {
  margin-top: 10px;
  font-size: 20px;
  margin: 10px auto;
  color: #000000;
  opacity: 0;
  animation: fadeInUp 0.5s ease forwards;
  font-family: 'Sarabun', sans-serif; /* ฟอนต์สำหรับคำอธิบาย */
}

/* CSS Animations */
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateX(-50px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

.fade-in {
  opacity: 0;
  animation: fadeIn 1s ease forwards;
}

/* Vue Transition for frame */
.slide-in-enter-active,
.slide-in-leave-active {
  transition: all 0.5s ease;
}

.slide-in-enter-from,
.slide-in-leave-to {
  opacity: 0;
  transform: translateX(-50px);
}
</style>