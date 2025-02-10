<!-- * 8.0 «computed-поля» — ещё один мощный инструмент во Vue, который позволяет делать какие-то калькуляции или трансформации, используя данные. -->
<!-- 8.1 Рассмотрим на следующем примере, например, мы хотим ограничить кол-во вводимых символов в строку ввода и, одновременно с этим, показывать юзеру сколько символов он ввёл из лимита. И это идеальный случай, когда мы можем использовать computed-поля. Во-первых, чтобы иметь возможность их использовать надо импортировать "computed" из "vue". -->
<script setup>
import { ref, computed } from 'vue'

const header = ref('Shopping List App')
const editing = ref(true)
const inputDisabled = ref(true)
// const excessiveCharacters = ref(false)
const items = ref([
  { id: 1, label: '10 party hats', purchased: false, highPriority: false },
  { id: 2, label: '2 board games', purchased: true, highPriority: false },
  { id: 3, label: '20 cups', purchased: false, highPriority: true },
])
const newItem = ref('')
const newItemHighPriority = ref(false)

// 8.2 Во-вторых, создадим функцию, которая будет считать кол-во введённых символов. Теперь, чтобы создать "computed-поле", которое обновляется только, когда обновится newItem, мы сделаем из безымянной стрелочной функции - функцию "computed".
const characterCount = computed(() => newItem.value.length)

// 8.4 Рассмотрим ещё один пример с computed-полем, в котором мы будем менять пункты списка местами, помещая более новые наверх. Создадим computed-поле "reversedItems", которое будет возвращать массив в обратном порядке. Используем для этого метод массивов "reverse".
// ! 8.5 Важно, чтобы computed-поле всегда возвращало какое-то значение!
// ? 8.6 Однако метод "reverse" изменяет данные массива, на котором он был вызван. А computed-поля только для трансформации отображения данных в тэмплэйте. Они не должны менять входящие данные. Потому нам нужно добавить spread-оператор "...", чтобы клонировать массив. Случайное изменение данных в исходных данных через computed-поля — это большой источник головной боли и багов в будущем, поэтому следует за этим внимательно следить.
const reversedItems = computed(() => [...items.value].reverse())

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
// Заменено на computed-поле:
// const excessiveCharsCount = () => {
//   if (newItem.value.length > 76) {
//     excessiveCharacters.value = true
//     return 76 - newItem.value.length
//   } else {
//     excessiveCharacters.value = false
//   }
// }
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
      <!--       <span v-if="excessiveCharacters" class="excessive-characters-num">
        {{ excessiveCharsCount() }}
      </span> -->
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
    <!-- 8.3 Добавим элемент <p>, в котором будет счётчик символов. ↑ -->
    <p v-if="editing" class="counter">{{ characterCount }}/200</p>
    <ul>
      <!-- 8.7 А теперь мы просто заменим массив "items" на изменённый с помощью computed-поля "reversedItems". -->
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
