<template>
  <div class="Contact">
    <h1 class="fade-in">Contact Me</h1>
    <p> </p>
    <!-- Frame สำหรับแสดงไอคอนและข้อมูลติดต่อ -->
    <transition name="slide-in">
      <div class="contact-frame">
        <div class="icon-section">
          <img src="@/assets/img/Contact.jpg" alt="My Icon" class="contact-icon fade-in" />
        </div>
        <div class="info-section">
          <p class="contact-info" v-for="(item, index) in contactItems" :key="index" :style="{ animationDelay: `${index * 0.2}s` }">
            <i :class="['fas', item.icon, 'icon']"></i>
            <span class="label">{{ item.label }}</span> {{ item.value }}
          </p>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'Contact',
  data() {
    return {
      email: '',
      phone: '',
      address: ''
    };
  },
  computed: {
    contactItems() {
      return [
        { label: "Email: ", value: this.email, icon: "fa-envelope" },
        { label: "Phone: ", value: this.phone, icon: "fa-phone" },
        { label: "Address: ", value: this.address, icon: "fa-map-marker-alt" }
      ];
    }
  },
  methods: {
    async loadContactInfo() {
      try {
        const response = await fetch('http://localhost:3000/contact/1');
        if (!response.ok) {
          throw new Error('โหลดข้อมูลล้มเหลว: ' + response.statusText);
        }

        const data = await response.json();
        this.email = data.email;
        this.phone = data.phone;
        this.address = data.address;
      } catch (error) {
        console.error('โหลดข้อมูลล้มเหลว:', error);
        this.email = 'example.email@domain.com';
        this.phone = '012-345-6789';
        this.address = '123 ถนนตัวอย่าง แขวงตัวอย่าง เขตตัวอย่าง กรุงเทพมหานคร 12345';
      }
    }
  },
  created() {
    this.loadContactInfo();
  }
};
</script>

<style scoped>
.Contact {
  padding: 20px;
  color: #ffffff;
  font-family: 'Sarabun', sans-serif; /* ฟอนต์พื้นฐาน */
}

.contact-frame {
  display: flex;
  gap: 20px;
  max-width: 1200px;
  width: 100%;
  background-color: #ffffff;
  margin: 20px auto;
  padding: 20px;
  border-radius: 20px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  transition: background-color 0.3s ease;
}

.icon-section {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
}

.contact-icon {
  width: 100%;
  max-width: 300px;
  height: auto;
  border-radius: 10px;
}

.info-section {
  flex: 2;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 15px;
}

h1 {
  font-family: 'Prompt', sans-serif; /* ฟอนต์สำหรับหัวข้อหลัก */
  font-size: 48px;
  font-weight: 700;
  color: #ffffff;
}

.contact-info {
  font-family: 'Sarabun', sans-serif; /* ฟอนต์สำหรับเนื้อหา */
  font-size: 24px;
  font-weight: 400;
  color: #000000;
  margin: 0;
  line-height: 1.6;
  display: flex;
  align-items: center;
  gap: 10px;
  opacity: 0;
  animation: fadeInUp 0.5s ease forwards;
  transition: transform 0.3s ease;
}

.contact-info:hover {
  transform: translateY(-10px);
}

.contact-info:hover .icon {
  transform: translateY(-10px);
}

.label {
  font-weight: 500; /* ปรับให้หนากว่าเนื้อหา */
  color: #333333;
}

.icon {
  font-size: 20px;
  color: #f7b43f;
  transition: transform 0.3s ease;
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

/* Vue Transition for contact-frame */
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