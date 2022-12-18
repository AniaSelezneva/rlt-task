<template>
  <div>
    <ul class="grid">
      <li v-for="gridCell in gridList" :key="gridCell" class="cell" :id="gridCell">
        <img 
          v-if="gridCell === matches[gridCell]?.position" 
          class="item" 
          draggable="true" 
          :id="matches[gridCell].name" 
          :key="matches[gridCell].name" 
          :alt="`${matches[gridCell].name} inventory item`" 
          :src="`${matches[gridCell].fileName}`" />
      </li>
    </ul>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import greenSvg from '../assets/green.svg'
import yellowSvg from '../assets/yellow.svg'
import purpleSvg from '../assets/purple.svg'

const trigger = ref()

const positions = JSON.parse(localStorage.getItem('positions')) || {};

const items = [{
    name: 'green',
    fileName: greenSvg,
    position: Number(positions.green) || 0,
  }, 
  {
    name: 'yellow',
    fileName: yellowSvg,
    position: Number(positions.yellow) || 1,
  },
  {
    name: 'purple',
    fileName: purpleSvg,
    position: Number(positions.purple) || 2,
  }
]

const gridList = [];
const matches = {};

for (let i = 0; i < 25; i++) {
  gridList.push(i);
  const idx = items.findIndex(item => item.position === i)
  if (idx >= 0) {
    matches[i] = items[idx]
  }
}

function dragstart_handler(ev) {
  trigger.value = ev.srcElement
}

function dragover_handler(ev) {
  ev.preventDefault();
  ev.dataTransfer.dropEffect = "move";
}

function drop_handler(ev) {
  ev.preventDefault();
  ev.target.appendChild(trigger.value);

  positions[trigger.value.id] = ev.target.id

  localStorage.setItem('positions', JSON.stringify(positions))
}

onMounted(() => {
  const cellElements = document.getElementsByClassName("cell");
  const draggableElements = document.getElementsByClassName('item');

  for (let i = 0; i < draggableElements.length; i++) {
    draggableElements[i].addEventListener("dragstart", dragstart_handler);
  }

  for (let i = 0; i < cellElements.length; i++) {
    cellElements[i].addEventListener("drop", drop_handler);
    cellElements[i].addEventListener("dragover", dragover_handler);
  }
})

</script>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}
.grid {
  background: #262626;
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-template-rows: repeat(5, 1fr);
  gap: 1px;
  height: 100vh;
}
.grid > li {
  outline: 1px solid #4D4D4D;
  display: flex;
  justify-content: center;
  align-items: center;
}
.item {
  width: 70%;
  height: 70%;
}
</style>
