<template>
  <div class="About">
    <h1 class="fade-in">About Me</h1>
    <p> </p>
    <!-- Container สำหรับจัดวาง frame-pic และ frame-text ข้างกัน -->
    <transition name="slide-in">
      <div class="frame-container">
        <div class="frame-pic">
          <div class="image-section">
            <img src="@/assets/img/HomePic.png" alt="Description of image" class="portfolio-image fade-in" />
          </div>
        </div>
        <div class="frame-text">
          <p class="image-description" v-for="(item, index) in infoItems" :key="index" :style="{ animationDelay: `${index * 0.2}s` }">
            <i :class="['fas', item.icon, 'icon']"></i>
            <span class="label">{{ item.label }}</span> {{ item.value }}
          </p>
        </div>
      </div>
    </transition>
    <button class="edit-button" @click="openEditModal">
      <i class="fas fa-edit"></i> แก้ไขข้อมูล
    </button>

    <!-- Modal สำหรับแก้ไขข้อมูลด้วย Bootstrap -->
    <transition name="modal">
      <div v-if="showModal" class="modal fade show" tabindex="-1" style="display: block;" aria-modal="true" role="dialog">
        <div class="modal-dialog modal-dialog-centered">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title">แก้ไขข้อมูล</h5>
              <button type="button" class="btn-close" @click="closeEditModal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
              <form @submit.prevent="saveInfo">
                <div class="mb-3">
                  <label for="name" class="form-label">ชื่อ:</label>
                  <input type="text" class="form-control" id="name" v-model="editedName" required />
                </div>
                <div class="mb-3">
                  <label for="studentId" class="form-label">รหัสนิสิต:</label>
                  <input type="text" class="form-control" id="studentId" v-model="editedStudentId" required />
                </div>
                <div class="mb-3">
                  <label for="major" class="form-label">สาขา:</label>
                  <input type="text" class="form-control" id="major" v-model="editedMajor" required />
                </div>
                <div class="mb-3">
                  <label for="oldSchool" class="form-label">โรงเรียนเก่า:</label>
                  <input type="text" class="form-control" id="oldSchool" v-model="editedOldSchool" required />
                </div>
                <div class="d-flex justify-content-between">
                  <button type="submit" class="btn btn-success">บันทึก</button>
                  <button type="button" class="btn btn-danger" @click="closeEditModal">ยกเลิก</button>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
    </transition>
    <!-- Backdrop -->
    <transition name="fade">
      <div v-if="showModal" class="modal-backdrop fade show"></div>
    </transition>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "",
      studentId: "",
      major: "",
      oldSchool: "",
      showModal: false,
      editedName: "",
      editedStudentId: "",
      editedMajor: "",
      editedOldSchool: ""
    };
  },
  computed: {
    infoItems() {
      return [
        { label: "Name: ", value: this.name, icon: "fa-user" },
        { label: "StudentID: ", value: this.studentId, icon: "fa-id-card" },
        { label: "Major: ", value: this.major, icon: "fa-graduation-cap" },
        { label: "OldSchool: ", value: this.oldSchool, icon: "fa-school" }
      ];
    }
  },
  methods: {
    openEditModal() {
      this.editedName = this.name;
      this.editedStudentId = this.studentId;
      this.editedMajor = this.major;
      this.editedOldSchool = this.oldSchool;
      this.showModal = true;
    },
    closeEditModal() {
      this.showModal = false;
    },
    async saveInfo() {
      this.name = this.editedName;
      this.studentId = this.editedStudentId;
      this.major = this.editedMajor;
      this.oldSchool = this.editedOldSchool;

      const updatedData = {
        id: 1,
        name: this.name,
        studentId: this.studentId,
        major: this.major,
        oldSchool: this.oldSchool
      };

      this.showModal = false;

      try {
        const response = await fetch('http://localhost:3000/profile/1', {
          method: 'PUT',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(updatedData)
        });

        if (!response.ok) {
          throw new Error('เกิดข้อผิดพลาดในการบันทึกข้อมูล');
        }

        const data = await response.json();
        console.log('บันทึกข้อมูลสำเร็จ:', data);
      } catch (error) {
        console.error('เกิดข้อผิดพลาด:', error);
      }
    },
    async loadFromJson() {
      try {
        const response = await fetch('http://localhost:3000/profile/1');
        if (!response.ok) {
          throw new Error('โหลดข้อมูลล้มเหลว');
        }

        const data = await response.json();
        this.name = data.name;
        this.studentId = data.studentId;
        this.major = data.major;
        this.oldSchool = data.oldSchool;
      } catch (error) {
        console.error('โหลดข้อมูลล้มเหลว:', error);
        this.name = "Theerachote  Sakaratnatee";
        this.studentId = "6630200691";
        this.major = "ComputerScience";
        this.oldSchool = "WatsongthamSchool";
      }
    }
  },
  created() {
    this.loadFromJson();
  }
};
</script>

<style scoped>
/* นำเข้าฟอนต์ */
@import url('https://fonts.googleapis.com/css2?family=Prompt:wght@400;700&family=Sarabun:wght@400;700&display=swap');

.About {
  padding: 20px;
  color: #ffffff;
  font-family: 'Sarabun', sans-serif; /* ฟอนต์หลักสำหรับเนื้อหา */
}

h1 {
  font-family: 'Prompt', sans-serif; /* ฟอนต์สำหรับหัวข้อ */
  font-weight: 700;
}

.frame-container {
  display: flex;
  gap: 20px;
  max-width: 1200px;
  width: 120%;
  margin: 20px auto;
}

.frame-pic {
  flex: 1;
  background-color: #ffffff;
  padding: 10px;
  border-radius: 0px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  transition: background-color 0.3s ease;
  display: flex;
  justify-content: center;
}

.frame-text {
  flex: 1.5;
  background-color: #ffffff;
  padding: 20px;
  border-radius: 20px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  transition: background-color 0.3s ease;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 400px;
}

.image-section {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.portfolio-image {
  width: 100%;
  max-width: 400px;
  height: auto;
  margin: 10px 0;
  border-radius: 10px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.image-description {
  margin: 0;
  font-size: 24px;
  color: #000000;
  line-height: 1.8;
  opacity: 0;
  animation: fadeInUp 0.5s ease forwards;
  display: flex;
  align-items: center;
  gap: 10px;
  transition: transform 0.3s ease;
  font-family: 'Sarabun', sans-serif; /* ฟอนต์สำหรับคำอธิบาย */
}

.image-description:hover {
  transform: translateY(-10px);
}

.label {
  font-weight: bold;
  color: #333333;
  font-family: 'Sarabun', sans-serif; /* ฟอนต์สำหรับ label */
}

.icon {
  font-size: 20px;
  color: #f7b43f;
  transition: transform 0.3s ease;
}

.image-description:hover .icon {
  transform: translateY(-10px);
}

.edit-button {
  padding: 10px 20px;
  font-size: 16px;
  color: #ffffff;
  background-color: #f7b43f;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: transform 0.3s ease, background-color 0.3s ease;
  display: flex;
  align-items: center;
  gap: 8px;
  margin: 20px;
  font-family: 'Prompt', sans-serif; /* ฟอนต์สำหรับปุ่ม */
}

.edit-button:hover {
  background-color: #0056b3;
  transform: scale(1.05);
}

.edit-button:active {
  transform: scale(0.95);
}

/* ปรับแต่งเพิ่มเติมสำหรับ Bootstrap */
.modal-content {
  border-radius: 10px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}

.modal-header {
  background-color: #f8f9fa;
  border-bottom: 1px solid #dee2e6;
}

.modal-title {
  font-size: 1.5rem;
  color: #343a40;
  font-family: 'Prompt', sans-serif; /* ฟอนต์สำหรับหัวข้อใน modal */
}

.modal-backdrop {
  opacity: 0.5;
}

.btn-close {
  filter: grayscale(100%);
}

.form-control {
  border-radius: 5px;
  font-family: 'Sarabun', sans-serif; /* ฟอนต์สำหรับ input */
}

.form-control:focus {
  border-color: #007bff;
  box-shadow: 0 0 0 0.25rem rgba(0, 123, 255, 0.25);
}

.form-label {
  font-family: 'Prompt', sans-serif; /* ฟอนต์สำหรับ label ในฟอร์ม */
}

.btn-success {
  background-color: #28a745;
  border-radius: 5px;
  font-family: 'Prompt', sans-serif; /* ฟอนต์สำหรับปุ่มบันทึก */
}

.btn-success:hover {
  background-color: #218838;
}

.btn-danger {
  background-color: #dc3545;
  border-radius: 5px;
  font-family: 'Prompt', sans-serif; /* ฟอนต์สำหรับปุ่มยกเลิก */
}

.btn-danger:hover {
  background-color: #c82333;
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

/* Vue Transition for frame-container */
.slide-in-enter-active,
.slide-in-leave-active {
  transition: all 0.5s ease;
}

.slide-in-enter-from,
.slide-in-leave-to {
  opacity: 0;
  transform: translateX(-50px);
}

/* Vue Transition for Modal */
.modal-enter-active,
.modal-leave-active {
  transition: all 0.3s ease;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
  transform: scale(0.9);
}

/* Vue Transition for Backdrop */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>