<template>
  <div class="about-container">
    <h3>Version 1.0.0</h3>
    <router-link to="/">Go Back</router-link>
  </div>
</template>

<script>
export default {
  name: "About",
};
</script>

<style scoped>
.about-container {
  text-align: center;
  border-top: 2px solid steelblue;
  border-bottom: 2px solid steelblue;
  margin: 10px;
  margin-top: 30px;
  padding: 10px 20px;
}
</style>