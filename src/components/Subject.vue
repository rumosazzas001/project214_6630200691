<template>
  <div class="Subject">
    <h1 class="fade-in">My Subject</h1>
    <p> </p>

    <!-- ฟอร์มสำหรับเพิ่มข้อมูล -->
    <transition name="slide-in">
      <div class="add-subject-form">
        <h3 class="fade-in"><i class="fas fa-plus-circle icon"></i> เพิ่มวิชาใหม่</h3>
        <form @submit.prevent="addSubject">
          <input v-model="newSubject.code" placeholder="รหัสวิชา" required />
          <input v-model="newSubject.name" placeholder="ชื่อวิชา" required />
          <select v-model.number="newSubject.credit" required>
            <option disabled value="">เลือกหน่วยกิต</option>
            <option v-for="credit in credits" :key="credit" :value="credit">{{ credit }}</option>
          </select>
          <select v-model="newSubject.grade" required>
            <option disabled value="">เลือกเกรด</option>
            <option v-for="grade in grades" :key="grade" :value="grade">{{ grade }}</option>
          </select>
          <button type="submit"><i class="fas fa-plus icon"></i> เพิ่มวิชา</button>
        </form>
      </div>
    </transition>

    <!-- ฟอร์มสำหรับแก้ไขข้อมูล -->
    <transition name="slide-in">
      <div class="edit-subject-form">
        <h3 class="fade-in"><i class="fas fa-edit icon"></i> แก้ไขวิชา</h3>
        <form @submit.prevent="updateSubject">
          <div class="select-row">
            <select v-model="selectedSubjectId" @change="loadSubjectToEdit" required>
              <option disabled value="">เลือกวิชาที่ต้องการแก้ไข</option>
              <option v-for="subject in subjects" :key="subject.id" :value="subject.id">
                {{ subject.code }} - {{ subject.name }}
              </option>
            </select>
          </div>
          <div v-if="editingSubject" class="edit-row">
            <input v-model="editingSubject.code" placeholder="รหัสวิชา" required />
            <input v-model="editingSubject.name" placeholder="ชื่อวิชา" required />
            <select v-model.number="editingSubject.credit" required>
              <option v-for="credit in credits" :key="credit" :value="credit">{{ credit }}</option>
            </select>
            <select v-model="editingSubject.grade" required>
              <option v-for="grade in grades" :key="grade" :value="grade">{{ grade }}</option>
            </select>
            <button type="submit"><i class="fas fa-save icon"></i> บันทึก</button>
          </div>
        </form>
      </div>
    </transition>

    <!-- ตารางแสดงข้อมูล (ไม่มีไอคอน) -->
    <transition name="slide-in">
      <section class="subject-container">
        <div class="subject-table">
          <table>
            <thead>
              <tr>
                <th>รหัสวิชา</th>
                <th>ชื่อวิชา</th>
                <th>หน่วยกิต</th>
                <th>เกรด</th>
                <th>การจัดการ</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="subject in subjects" :key="subject.id">
                <td>{{ subject.code }}</td>
                <td>{{ subject.name }}</td>
                <td>{{ subject.credit }}</td>
                <td>{{ subject.grade }}</td>
                <td>
                  <button @click="deleteSubject(subject.id)" class="delete-btn"><i class="fas fa-trash icon"></i> ลบ</button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </section>
    </transition>
  </div>
</template>

<script>
export default {
  data() {
    return {
      subjects: [],
      newSubject: {
        code: '',
        name: '',
        credit: '',
        grade: ''
      },
      selectedSubjectId: '',
      editingSubject: null,
      credits: [1, 2, 3],
      grades: ['A', 'A-', 'B+', 'B', 'B-', 'C+', 'C', 'C-', 'D+', 'D', 'D-', 'F']
    };
  },
  mounted() {
    this.fetchSubjects();
  },
  methods: {
    async fetchSubjects() {
      try {
        const response = await fetch('http://localhost:3000/subjects');
        if (!response.ok) throw new Error('Network response was not ok');
        const data = await response.json();
        this.subjects = data;
      } catch (error) {
        console.error('Error fetching subjects:', error);
        alert('เกิดข้อผิดพลาดในการดึงข้อมูล: ' + error.message);
      }
    },

    async addSubject() {
      try {
        const response = await fetch('http://localhost:3000/subjects', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(this.newSubject)
        });
        if (!response.ok) throw new Error('Network response was not ok');
        const data = await response.json();
        this.subjects.push(data);
        this.newSubject = { code: '', name: '', credit: '', grade: '' };
        alert('เพิ่มวิชาสำเร็จ!');
      } catch (error) {
        console.error('Error adding subject:', error);
        alert('เกิดข้อผิดพลาดในการเพิ่มวิชา: ' + error.message);
      }
    },

    loadSubjectToEdit() {
      const subject = this.subjects.find(s => s.id === this.selectedSubjectId);
      if (subject) {
        this.editingSubject = { ...subject };
      } else {
        this.editingSubject = null;
      }
    },

    async updateSubject() {
      if (!this.editingSubject) return;
      try {
        const response = await fetch(`http://localhost:3000/subjects/${this.editingSubject.id}`, {
          method: 'PUT',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(this.editingSubject)
        });
        if (!response.ok) throw new Error('Network response was not ok');
        const data = await response.json();
        const index = this.subjects.findIndex(subject => subject.id === data.id);
        if (index !== -1) {
          this.subjects.splice(index, 1, data);
        }
        this.editingSubject = null;
        this.selectedSubjectId = '';
        alert('แก้ไขวิชาสำเร็จ!');
      } catch (error) {
        console.error('Error updating subject:', error);
        alert('เกิดข้อผิดพลาดในการแก้ไขวิชา: ' + error.message);
      }
    },

    async deleteSubject(id) {
      if (confirm('คุณแน่ใจหรือไม่ว่าต้องการลบวิชานี้?')) {
        try {
          const response = await fetch(`http://localhost:3000/subjects/${id}`, {
            method: 'DELETE'
          });
          if (!response.ok) throw new Error('Network response was not ok');
          this.subjects = this.subjects.filter(subject => subject.id !== id);
          if (this.editingSubject && this.editingSubject.id === id) {
            this.editingSubject = null;
            this.selectedSubjectId = '';
          }
          alert('ลบวิชาสำเร็จ!');
        } catch (error) {
          console.error('Error deleting subject:', error);
          alert('เกิดข้อผิดพลาดในการลบวิชา: ' + error.message);
        }
      }
    }
  }
};
</script>

<style scoped>
/* นำเข้าฟอนต์ */
@import url('https://fonts.googleapis.com/css2?family=Prompt:wght@400;700&family=Sarabun:wght@400;700&display=swap');

.Subject {
  padding: 20px;
  color: #ffffff;
  font-family: 'Sarabun', sans-serif; /* ฟอนต์หลักสำหรับเนื้อหา */
}

h1 {
  font-family: 'Prompt', sans-serif; /* ฟอนต์สำหรับหัวข้อหลัก */
  font-weight: 700;
}

h3 {
  font-family: 'Prompt', sans-serif; /* ฟอนต์สำหรับหัวข้อย่อย */
  font-weight: 700;
  display: flex;
  align-items: center;
  gap: 10px; /* ระยะห่างระหว่างไอคอนและข้อความ */
}

.add-subject-form, .edit-subject-form {
  margin-bottom: 20px;
  background-color: #ffffff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  max-width: 1500px;
  width: 100%;
  margin-left: auto;
  margin-right: auto;
}

.add-subject-form h3, .edit-subject-form h3 {
  color: #333333;
  margin-bottom: 15px;
}

.add-subject-form form {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
}

.edit-subject-form form {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.select-row, .edit-row {
  display: flex;
  gap: 10px;
  align-items: center;
  justify-content: space-between;
}

.add-subject-form input,
.add-subject-form select,
.edit-subject-form input,
.edit-subject-form select {
  padding: 8px;
  border: 1px solid #dee2e6;
  border-radius: 5px;
  font-size: 16px;
  color: #333333;
  width: 280px;
  box-sizing: border-box;
  font-family: 'Sarabun', sans-serif; /* ฟอนต์สำหรับ input และ select */
}

.add-subject-form button, .edit-subject-form button {
  padding: 8px 15px;
  background-color: #f7b43f;
  color: #ffffff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  width: 280px;
  font-family: 'Prompt', sans-serif; /* ฟอนต์สำหรับปุ่ม */
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px; /* ระยะห่างระหว่างไอคอนและข้อความ */
}

.add-subject-form button:hover, .edit-subject-form button:hover {
  background-color: #e0a038;
}

.subject-container {
  margin-top: 20px;
  background-color: #ffffff;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  padding: 20px;
  max-width: 1500px;
  margin-left: auto;
  margin-right: auto;
}

.subject-table {
  width: 100%;
  background-color: #ffffff;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  overflow-x: auto;
  border: 2px solid #f7b43f;
}

table {
  width: 100%;
  border-collapse: collapse;
}

thead {
  background-color: #f7b43f;
  color: #ffffff;
}

th {
  padding: 12px 15px;
  text-align: center;
  border-bottom: 1px solid #dee2e6;
  font-family: 'Prompt', sans-serif; /* ฟอนต์สำหรับหัวตาราง */
  font-size: 18px;
  font-weight: bold;
}

td {
  padding: 12px 15px;
  text-align: center;
  border-bottom: 1px solid #dee2e6;
  font-family: 'Sarabun', sans-serif; /* ฟอนต์สำหรับเนื้อหาในตาราง */
  font-size: 16px;
  color: #333333;
}

tbody tr:nth-child(even) {
  background-color: #f8f9fa;
}

.delete-btn {
  padding: 5px 10px;
  margin: 0 5px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  background-color: #dc3545;
  color: #ffffff;
  font-family: 'Prompt', sans-serif; /* ฟอนต์สำหรับปุ่มลบ */
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px; /* ระยะห่างระหว่างไอคอนและข้อความ */
}

.delete-btn:hover {
  background-color: #c82333;
}

.icon {
  font-size: 18px;
  color: inherit; /* ให้ไอคอนใช้สีเดียวกับข้อความ */
}

/* CSS Animations */
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

/* Vue Transition */
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