<template>
  <div id="app">
    <div class="sidebar">
      <button @click="currentSection = 'home'">
        <span class="dot"></span>
        <span class="number">01</span> Home
      </button>
      <button @click="currentSection = 'about'">
        <span class="dot"></span>
        <span class="number">02</span> About Me
      </button>
      <button @click="currentSection = 'Subject'">
        <span class="dot"></span>
        <span class="number">03</span> Subject
      </button>
      <button @click="currentSection = 'Contact'">
        <span class="dot"></span>
        <span class="number">03</span> Contact
      </button>
    </div>
    <div class="content">
      <component :is="currentSectionComponent" />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentSection: 'home',
    };
  },
  computed: {
    currentSectionComponent() {
      const components = {
        home: 'HomeComponent',
        about: 'AboutComponent',
        Subject: 'SubjectComponent',
        Contact: 'Contact'
      };
      return components[this.currentSection] || 'HomeComponent';
    },
  },
};
</script>

<style>
#app {
  display: flex;
}

.sidebar {
  width: 200px;
  background-color: #1a1a1a;
  color: white;
  position: fixed;
  height: 100%;
  padding: 20px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 30px; 
}

.sidebar button {
  color: #cccccc;
  background: none;
  border: none;
  text-align: left;
  width: 100%;
  padding: 10px 0;
  cursor: pointer;
  transition: color 0.3s;
  font-size: 18px;
  display: flex;
  align-items: center;
}

.sidebar button:hover {
  color: #ffffff;
}

.sidebar .dot {
  display: inline-block;
  width: 6px;
  height: 6px;
  background-color: #666666;
  border-radius: 50%;
  margin-right: 10px;
}

.sidebar .number {
  display: inline-block;
  width: 30px;
  color: #666666;
  font-size: 14px;
  margin-right: 10px;
}

.content {
  margin-left: 220px;
  padding: 20px;
  flex-grow: 1;
}
</style>