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

// * 5.0 Чтобы сохранить более чистый и лаконичный тэмплэйт, а также избегать повторения кода и иметь возможность писать более одной строчки JS-кода используют методы. Напишем метод для добавления записи в список.
// ? 5.1 Однако стоит помнить одну вещь, что в отличии от тэмплэйта, когда мы пишем внутри секции "script", то чтобы получить доступ к значению какой-то реактивной переменной (ref) — нужно использовать специальное свойство "value" (т.к. Vue использует прокси для реактивных данных). Для массивов и объектов это работает отлично, однако не работает для примитивных типов данных (строки, числа и булево значение). Поэтому, когда мы вызываем ref функцию, то возвращается реактивный объект с данными, которые хранятся в свойстве "value".
// 5.3 Также имеет смысл очищать нашу строку ввода, после добавления записи в список и т.к. значение синхронизировано с реактивной переменной при помощи "v-model", то мы просто присвоим пустую строку самой переменной в конце метода, что также очистит и строку ввода.
const saveItem = () => {
  items.value.push({ id: items.value.length + 1, label: newItem.value })
  newItem.value = ''
}
</script>

<template>
  <div>
    <h1>{{ header }}</h1>
    <!-- 5.2 Теперь мы можем использовать этот метод в обработчике событий. И в отличии от Vanilla JS во Vue "()" опциональны, когда вызываем методы — без них можно обойтись. ↑  -->
    <form @:submit.prevent="saveItem" class="add-item-form">
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
</template>
