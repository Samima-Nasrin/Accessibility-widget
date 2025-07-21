<template>
  <div class="floating-button-wrapper">
    <button class="floating-button" @click="toggleMenu"><i class="fas fa-universal-access"></i></button>
    <MenuPanel v-if="menuOpen" @close="toggleMenu" />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import MenuPanel from './MenuPanel.vue';
const menuOpen = ref(false);
const toggleMenu = () => menuOpen.value = !menuOpen.value;
</script>

<style scoped>
.floating-button-wrapper {
  position: fixed;
  bottom: 20px;
  right: 20px;
}
.floating-button {
  width: 75px;
  height: 75px;
  border-radius: 50%;
  background-color: #675bda;
  color: white;
  border: none;
  font-size: 24px;
  cursor: pointer;
  box-shadow: 0 4px 6px rgba(131, 133, 255, 0.7);
}
.floating-button i {
  font-size: 40px;
  color: rgb(238, 234, 234); /* or inherit depending on theme */
}
</style>