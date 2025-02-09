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
</script>

<template>
  <div>
    <h1>{{ header }}</h1>
    <!-- 4.2 Мы можем чуточку сократить код, если заменим div на form и передадим форме слушатель события "submit", тогда можно избавиться от слушателя события кнопки и инпута. Однако не нужно забывать модификатор "prevent", который отменит стандартное поведение после события "submit" - перезагрузку страницы. -->
    <form
      v-on:submit.prevent="items.push({ id: items.length + 1, label: newItem })"
      class="add-item-form"
    >
      <!-- <input
        v-model.trim="newItem"
        v-on:keyup.enter="items.push({ id: items.length + 1, label: newItem })"
        type="text"
        placeholder="Add an item"
      /> -->
      <input v-model.trim="newItem" type="text" placeholder="Add an item" />
      <label>
        <!-- 4.1 Было бы классно также добавлять записи по нажатию клавиши "Enter" на клавиатуре, это можно также сделать с помощью "v-on" и событие "keyup" и модификатором будет название клавиши "enter". Добавим его инпуту. -->
        <input v-model="newItemHighPriority" type="checkbox" />
        High Priority
      </label>
      <!-- * 4.0 Теперь нам нужна кнопка, чтобы сохранять новые пункты в массив пунктов списка покупок, а заодно увидеть слушатель событий в действии во Vue 3. С этим работает директива "v-on" (или сокращённо "@"). И событие у нас будет "click", а запускать будет метод "push", добавляющий в массив новый объект с пунктом списка покупок. В label у нас поместится содержимое переменной, привязанной к инпуту "newItem", а для генерации уникального id используем длину массива "items" + 1. -->
      <button class="btn btn-primary">Save Item</button>
    </form>
    <ul>
      <li v-for="({ id, label }, index) in items" :key="id">{{ ++index }}: {{ label }}</li>
    </ul>
  </div>
</template>
