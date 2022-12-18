<template>
  <div class="description">
    <div class="img_container">
        <img 
            :key="name" 
            :alt="`${name} inventory item`" 
            :src="src" />
    </div>
    <h3>{{props.name}}</h3>
    <p>{{props.desctiption}}</p>
    <button class="close_btn" @click="$emit('close')">
        <img 
          alt="close description" 
          :src="crossSvg" />
    </button>
    <button class="delete_btn" @click="$emit('delete', props.name)">
        Удалить предмет
    </button>
  </div>
</template>

<script setup>
import { defineProps } from 'vue'
import crossSvg from '../assets/cross.svg'
const props = defineProps(['name', 'desctiption', 'src'])

</script>

<style scoped>
.description {
    position: absolute;
    right: 0;
    top: 0;
    min-width: 250px;
    margin-top: 23px;
    height: calc(100vh - 30px);
    width: 50vw;
    background: rgba(38, 38, 38, 0.5);
    border: 1px solid #4D4D4D;
    color: #4D4D4D;
    backdrop-filter: blur(8px);
    border-top-right-radius: 12px;
    border-bottom-right-radius: 12px;
}
.img_container {
    height: 500px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-bottom: 1px solid #4D4D4D;
    margin-left: 15px;
    margin-right: 15px;
}
.img_container > img {
    min-height: 50%;
}
.close_btn {
    color: #FFFFFF;
    position: absolute;
    top: 14px;
    right: 14px;
    background: none;
    border: none;
    width: 12px;
    height: 12px;
    display: flex;
    justify-content: center;
    cursor: pointer;
}
.delete_btn {
    position: absolute;
    width: 220px;
    height: 39px;
    background: #FF8888;
    border-radius: 8px;
    border: none;
    bottom: 18px;
    cursor: pointer;
    font-weight: 400;
    font-size: 14px;
    color: #FFFFFF;
    font-family: 'SF Pro Display';
    right: 50%;
    transform: translate(50%, 0);
}
</style>
