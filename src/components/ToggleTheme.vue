<template>
 <label class="switch">
  <input type="checkbox" :onChange="checkbox_handler">
  <span class="slider" />
</label>
</template>

<script setup>
import { onMounted, ref } from 'vue'
const isDarkMode = ref(true)
const appEl = document.getElementById('app');

function checkbox_handler() {
  isDarkMode.value = !isDarkMode.value;
  if (isDarkMode.value) {
    appEl.classList.add('dark');
  } else {
    appEl.classList.remove('dark');
  }
}
onMounted(() => {
    appEl.classList.add('dark');
})
</script>

<style scoped>
.switch {
  position: absolute;
  left: 8px;
  top: 8px;
  display: inline-block;
  width: 60px;
  height: 34px;
}
.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}
.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: .4s;
  transition: .4s;
  border-radius: 34px;
}
.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
  border-radius: 50%;
}
input:checked + .slider {
  background-color: #4D4D4D;
}
input:focus + .slider {
  box-shadow: 0 0 1px #4D4D4D;
}
input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}
</style>
