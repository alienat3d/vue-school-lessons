<script setup>
import { ref, computed } from 'vue'

// REFS
const header = ref('Shopping List App')
const editing = ref(true)
const inputDisabled = ref(true)
const items = ref([
  { id: 1, label: '10 party hats', purchased: false, highPriority: false },
  { id: 2, label: '2 board games', purchased: true, highPriority: false },
  { id: 3, label: '20 cups', purchased: false, highPriority: true },
])
const newItem = ref('')
const newItemHighPriority = ref(false)

// COMPUTED-FIELDS
const characterCount = computed(() => newItem.value.length)
const reversedItems = computed(() => [...items.value].reverse())

// METHODS
const saveItem = () => {
  items.value.push({
    id: items.value.length + 1,
    label: newItem.value,
    highPriority: newItemHighPriority.value,
  })
  newItem.value = ''
  newItemHighPriority.value = ''
}

const doEdit = (e) => {
  editing.value = e
  newItem.value = ''
  newItemHighPriority.value = ''
}

const disableInput = () =>
  newItem.value.length === 0 || newItem.value.length > 76
    ? (inputDisabled.value = true)
    : (inputDisabled.value = false)

const togglePurchased = (item) => (item.purchased = !item.purchased)
</script>

<template>
  <div>
    <div class="header">
      <h1>{{ header }}</h1>
      <button v-if="editing" @:click="doEdit(false)" class="btn">Cancel</button>
      <button v-else @:click="doEdit(true)" class="btn btn-primary">Add Item</button>
    </div>
    <form v-if="editing" @:submit.prevent="saveItem" class="add-item-form">
      <input
        v-model.trim="newItem"
        @:input="disableInput"
        @input="excessiveCharsCount"
        type="text"
        placeholder="Add an item"
      />
      <label>
        <input v-model="newItemHighPriority" type="checkbox" />
        High Priority
      </label>
      <button class="btn btn-primary" :disabled="inputDisabled">Save Item</button>
    </form>
    <p v-if="editing" class="counter">{{ characterCount }}/200</p>
    <ul>
      <li
        v-for="({ id, label, purchased, highPriority }, index) in reversedItems"
        @click="togglePurchased(items[index - 1])"
        :key="id"
        :class="{ strikeout: purchased, priority: highPriority }"
        class="some-static-class"
      >
        {{ ++index }}: {{ label }}
      </li>
    </ul>
  </div>
  <p v-if="!items.length">Nothing to see here yet</p>
</template>

<style scoped>
.excessive-characters-num {
  position: absolute;
  left: -22px;
  color: red;
}
ul {
  margin-top: 20px;
}
ul li {
  user-select: none;
}
</style>
