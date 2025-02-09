<script setup>
import { ref } from 'vue'

const header = ref('Shopping List App')
const items = ref([
  { id: 1, label: '10 party hats' },
  { id: 2, label: '2 board games' },
  { id: 3, label: '20 cups' },
])
const newItem = ref('')
const newItemHighPriority = ref(false)
const iceCreamFlavors = ref([])
</script>

<template>
  <div>
    <h1>{{ header }}</h1>
    <input v-model.trim="newItem" type="text" placeholder="Add an item" />
    <label>
      <input v-model="newItemHighPriority" type="checkbox" />
      High Priority
    </label>
    <label>
      <input v-model="iceCreamFlavors" type="checkbox" value="vanilla" />
      Vanilla
    </label>
    <label>
      <input v-model="iceCreamFlavors" type="checkbox" value="chocolate" />
      Chocolate
    </label>
    <label>
      <input v-model="iceCreamFlavors" type="checkbox" value="strawberry" />
      Strawberry
    </label>
    {{ iceCreamFlavors }}
    <ul>
      <li v-for="({ id, label }, index) in items" :key="id">{{ ++index }}: {{ label }}</li>
    </ul>
  </div>
</template>

<style></style>
