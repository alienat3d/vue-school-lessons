<script setup>
import { ref } from 'vue'

const header = ref('Shopping List App')
// 6.1.0 Мы будем скрывать форму, когда не используем её. И нам понадобится ещё одна реактивная переменная "editing", которая по умолчанию пример значение false. ↓
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
// 6.1.3 Для удобства создадим новый метод, который будет заниматься скрытием и показом кнопок и формы "doEdit". Аргументом будет "e" (от 'editing'), а также мы будем очищать значение newItem, когда этот метод сработает.
const doEdit = (e) => {
  editing.value = e
  newItem.value = ''
}
</script>

<template>
  <div>
    <div class="header">
      <h1>{{ header }}</h1>
      <!-- 6.1.2 Мы будем показывать кнопку "Cancel", когда в режиме редактирования и "Add Item", когда нет. -->
      <!-- 6.1.4 А далее мы передаём значения true или false в метод "doEdit" в зависимости от кнопки. -->
      <button v-if="editing" @:click="doEdit(false)" class="btn">Cancel</button>
      <button v-else @:click="doEdit(true)" class="btn btn-primary">Add Item</button>
    </div>
    <!-- 6.1.1 Ну, а форма примет директиву "v-if" и ещё нам понадобятся кнопки, чтобы переключать это состояние. ↑ -->
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
  <!-- * 6.0 Часто бывают ситуации, когда, в зависимости от определённых условий, показывать либо одну часть вёрстки, либо другую. Здесь выручают директивы "v-if" & "v-else". Мы можем использовать их в нашем приложении для случая, когда в массиве нет ещё ни одной записи. Это хороший пример показа одного элемента по условию, но как быть, если у нас два разных состояния вёрстки? ↑ -->
  <p v-if="!items.length">Nothing to see here yet</p>
</template>
