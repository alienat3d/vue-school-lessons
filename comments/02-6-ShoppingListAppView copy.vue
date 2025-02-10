<script setup>
import { ref } from 'vue'

const header = ref('Shopping List App')
const editing = ref(true)
const inputDisabled = ref(true)
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
const disableInput = () => {
  if (newItem.value.length === 0 || newItem.value.length > 9) {
    return (inputDisabled.value = true)
  } else {
    return (inputDisabled.value = false)
  }
}
</script>

<template>
  <div>
    <div class="header">
      <h1>{{ header }}</h1>
      <button v-if="editing" @:click="doEdit(false)" class="btn">Cancel</button>
      <button v-else @:click="doEdit(true)" class="btn btn-primary">Add Item</button>
    </div>
    <!-- * 6.0 Во Vue можно делать динамические HTML-атрибуты с помощью v-bind (сокр. ":") директивы можно связать их с рефами. Рассмотрим это на элементе ссылки. Так можно связывать любые атрибуты с данными. -->
    <!-- <a :href="`https://${newItem}`">Dynamic Link</a> -->
    <form v-if="editing" @:submit.prevent="saveItem" class="add-item-form">
      <input v-model.trim="newItem" @:input="disableInput" type="text" placeholder="Add an item" />
      <label>
        <input v-model="newItemHighPriority" type="checkbox" />
        High Priority
      </label>
      <!-- 6.1 А у кнопки будет полезным связать атрибут "disabled" с данными из newItem, чтобы кнопку можно было нажать только тогда, когда в строку ввода что-то ввели. -->
      <!-- 6.2 Или можно пойти дальше и ограничить также кол-во допустимых для ввода символов, но для этого сделаем дополнительный метод. -->
      <button class="btn btn-primary" :disabled="inputDisabled">Save Item</button>
    </form>
    <ul>
      <li v-for="({ id, label }, index) in items" :key="id">{{ ++index }}: {{ label }}</li>
    </ul>
  </div>
  <p v-if="!items.length">Nothing to see here yet</p>
</template>
