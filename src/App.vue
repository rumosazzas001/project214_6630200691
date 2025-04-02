<template>
  <div id="app">
    <Navbar @change-section="currentSection = $event" />
    <div class="app-container">
      <div class="sidebar">
        <button @click="currentSection = 'home'">Home</button>
        <button @click="currentSection = 'about'">About Me</button>
        <button @click="currentSection = 'subject'">Subject</button>
        <button @click="currentSection = 'contact'">Contact</button>
      </div>
      <div class="content">
        <component :is="currentSectionComponent" />
      </div>
    </div>
  </div>
</template>

<script>
import Home from './components/Home.vue';
import About from './components/About.vue';
import Subject from './components/Subject.vue';
import Contact from './components/Contact.vue';
import Navbar from './components/Navbar.vue';

export default {
  name: 'App',
  components: {
    Navbar,
    Home,
    About,
    Subject,
    Contact
  },
  data() {
    return {
      currentSection: 'home',
    };
  },
  computed: {
    currentSectionComponent() {
      return {
        home: 'Home',
        about: 'About',
        subject: 'Subject',
        contact: 'Contact'
      }[this.currentSection] || 'Home';
    },
  },
};
</script>

<style scoped>
#app {
  display: flex;
  flex-direction: column;
  font-family: 'Sarabun', sans-serif; /* ฟอนต์พื้นฐาน */
}

.app-container {
  display: flex;
}

.sidebar {
  width: 200px;
  background-color: #f7b43f;
  padding: 20px;
  height: 100vh;
  position: fixed;
  top: 0;
  left: 0;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.sidebar button {
  padding: 10px;
  font-family: 'Prompt', sans-serif; /* ฟอนต์สำหรับปุ่ม */
  font-size: 16px;
  font-weight: 600; /* หนาเพื่อความเด่น */
  color: #000000;
  background-color: #fffcfc;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.sidebar button:hover {
  background-color: #c88e2f;
}

.content {
  margin-left: 220px;
  padding: 20px;
  flex-grow: 1;
  min-height: 100vh;
}
</style>