<template>
  <div>
    <ToggleTheme />
    <ul class="grid">
      <li v-for="gridCell in gridList" :key="gridCell" class="cell" :id="gridCell">
        <img 
          v-if="gridCell === matches[gridCell]?.position" 
          class="item" 
          draggable="true"
          :id="matches[gridCell].name" 
          :key="matches[gridCell].name" 
          :alt="`${matches[gridCell].name} inventory item`" 
          :src="matches[gridCell].fileName" />
        <ItemDescription 
          v-if=" matches[gridCell] && clicked === matches[gridCell].name" 
          :name="matches[gridCell]?.name"
          :desctiption="matches[gridCell]?.desctiption"
          :src="matches[gridCell]?.fileName"
          @close="closeDescription"
          @delete="deleteItem"
        />
      </li>
    </ul>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import ItemDescription from './ItemDescription.vue'
import ToggleTheme from './ToggleTheme.vue'
import greenSvg from '../assets/green.svg'
import yellowSvg from '../assets/yellow.svg'
import purpleSvg from '../assets/purple.svg'
import cursorGrab from '../assets/cursor_grab.svg'

const trigger = ref()
const clicked = ref()

const positions = JSON.parse(localStorage.getItem('positions')) || {};

const items = [{
    name: 'green',
    fileName: greenSvg,
    position: positions.green != undefined ? Number(positions.green) : 0,
    desctiption: 'Nunc quis convallis nulla. In ac auctor elit. Curabitur luctus nibh non risus maximus pellentesque. Nunc fermentum ante vel elit auctor, a venenatis enim convallis. Vivamus eleifend urna quis augue ornare ornare. Nunc vehicula hendrerit elit sed dignissim. Nam mollis augue massa, bibendum fringilla turpis iaculis id. Quisque ipsum libero, efficitur a sem in, viverra faucibus tellus. Nunc quis libero accumsan, pharetra neque lacinia, aliquam lectus.'
  }, 
  {
    name: 'yellow',
    fileName: yellowSvg,
    position: positions.yellow != undefined ? Number(positions.yellow) : 1,
    desctiption: 'Proin porta risus vitae massa ornare, at interdum sapien commodo. Donec aliquam lorem nulla.'
  },
  {
    name: 'purple',
    fileName: purpleSvg,
    position: positions.purple != undefined ? Number(positions.purple) : 2,
    desctiption: 'In vestibulum ante ut magna aliquet accumsan. Vestibulum est ex, vulputate eget enim vel, faucibus interdum risus.'
  }
]

const gridList = [];
const matches = {};

let grabImg = new Image();
grabImg.src = cursorGrab;

for (let i = 0; i < 25; i++) {
  gridList.push(i);
  const idx = items.findIndex(item => item.position === i)
  if (idx >= 0) {
    matches[i] = items[idx]
  }
}

function dragstart_handler(ev) {
  trigger.value = ev.srcElement;
  ev.dataTransfer.setDragImage(grabImg, 0, 0);
}

function dragover_handler(ev) {
  ev.preventDefault();
  ev.dataTransfer.dropEffect = "move";
}

function drop_handler(ev) {
  ev.preventDefault();

  if (ev.target.id && gridList.includes(Number(ev.target.id))) {
    ev.target.appendChild(trigger.value);
  }

  positions[trigger.value.id] = ev.target.id

  localStorage.setItem('positions', JSON.stringify(positions))
}

function click_handler(ev) {
  clicked.value = ev.target.id
}

function closeDescription() {
  clicked.value = undefined
}

function deleteItem(name) {
  clicked.value = undefined

  delete positions[name]
  localStorage.setItem('positions', JSON.stringify(positions))

  const key = Object.keys(matches).find((key) => matches[key].name === name);

  delete matches[key]
}

onMounted(() => {
  const cellElements = document.getElementsByClassName("cell");
  const draggableElements = document.getElementsByClassName('item');

  for (let i = 0; i < draggableElements.length; i++) {
    draggableElements[i].addEventListener("dragstart", dragstart_handler);
    draggableElements[i].addEventListener("click", click_handler);
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
  background: transparent;
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-template-rows: repeat(5, 1fr);
  gap: 1px;
  height: calc(100vh - 30px);
  width: calc(100vh - 30px);
  border-radius: 12px;
  outline: 1px solid #4D4D4D;
}
.grid > li {
  outline: 1px solid #4D4D4D;
  display: flex;
  justify-content: center;
  align-items: center;
}
.grid > li:nth-child(1),
.grid > li:nth-child(5),
.grid > li:nth-child(21),
.grid > li:nth-child(25) {
  outline: none;
} 
.item {
  width: 70%;
  height: 70%;
  cursor: url("../assets/cursor_clarity.svg") 18 21, pointer;
}
</style>
