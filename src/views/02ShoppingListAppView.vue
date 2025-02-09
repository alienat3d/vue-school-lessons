<script setup>
import { ref } from 'vue'

const header = ref('Shopping List App')
const editing = ref(false)
const items = ref([
  // { id: 1, label: '10 party hats' },
  // { id: 2, label: '2 board games' },
  // { id: 3, label: '20 cups' },
])
const newItem = ref('')
const newItemHighPriority = ref(false)

const saveItem = () => {
  items.value.push({ id: items.value.length + 1, label: newItem.value })
  newItem.value = ''
}
const doEdit = (e) => {
  editing.value = e
  newItem.value = ''
}
</script>

<template>
  <div>
    <div class="header">
      <h1>{{ header }}</h1>
      <button v-if="editing" @:click="doEdit(false)" class="btn">Cancel</button>
      <button v-else @:click="doEdit(true)" class="btn btn-primary">Add Item</button>
    </div>
    <form v-if="editing" @:submit.prevent="saveItem" class="add-item-form">
      <input v-model.trim="newItem" type="text" placeholder="Add an item" />
      <label>
        <input v-model="newItemHighPriority" type="checkbox" />
        High Priority
      </label>
      <button class="btn btn-primary">Save Item</button>
    </form>
    <ul>
      <li v-for="({ id, label }, index) in items" :key="id">{{ ++index }}: {{ label }}</li>
    </ul>
  </div>
  <p v-if="!items.length">Nothing to see here yet</p>
</template>
