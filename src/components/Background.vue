<template>
  <div class="background-container">
    <h1>Welcome to My Vue App</h1>
  </div>
</template>

<script>
export default {
  name: "BackgroundComponent",
};
</script>

<style scoped>
.background-container {
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background-image: url('@/assets/img/BGimg.jpg');
  background-size: cover;
  background-position: center;
}
</style>
