<script setup>
import { ref } from 'vue'

const header = ref('Shopping List App')
const editing = ref(true)
const inputDisabled = ref(true)
const excessiveCharacters = ref(false)
// 7.1 Естественно нам нужно теперь свойство "purchased" для каждого айтема.
// 7.2.0 Также можно добавить и другие индикации, например, чтобы выделять задачи с "высоким приоритетом". ↓
const items = ref([
  { id: 1, label: '10 party hats', purchased: false, highPriority: false },
  { id: 2, label: '2 board games', purchased: true, highPriority: false },
  { id: 3, label: '20 cups', purchased: false, highPriority: true },
])
const newItem = ref('')
const newItemHighPriority = ref(false)

// 7.7 Раз уж мы стилизуем элементы, добавим также логику стилизации новых пунктов списка, которые были помечены чекбоксом "High Priority". Для этого добавим в новый объект, который пушит наш "saveItem" метод ещё одно свойство "highPriority". А также нужно сбросить значение чекбокса, как это делали со значением строки ввода.
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
const excessiveCharsCount = () => {
  if (newItem.value.length > 76) {
    excessiveCharacters.value = true
    return 76 - newItem.value.length
  } else {
    excessiveCharacters.value = false
  }
}
// 7.6.0 А теперь создадим новый метод "togglePurchased", который будет переключать пункты списка в состояние уже купленных с соответствующей стилизацией. Аргументом передадим конкретный item и изменим ему свойство "purchased" на противоположное. ↓
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
      <span v-if="excessiveCharacters" class="excessive-characters-num">{{
        excessiveCharsCount()
      }}</span>
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
    <ul>
      <!-- * 7.0 Особый случай занимает привязка классов, потому, что мы можем использовать дополнительно данные, чтобы определять когда применять какие классы. Чтобы это увидеть добавим стилизацию зачёркивания к некоторым пунктам в нашем листе. Для этого используем спец. синтаксис :class="{класс, который хотим вставлять}". Значение "purchased" будет булевым и показывать куплен ли уже товар или ещё нет. Мы также можем деструктурировать его из item. ↑ -->
      <!-- 7.2.1 Также добавим класс и для задач с высоким приоритетом. -->
      <!-- 7.3 Но как теперь на li’шки повесить обычные статические CSS-классы? Мы просто добавим их к элементу без ":" вначале 'class="some-static-class"'. -->
      <!-- 7.6.1 Добавим слушатель события клика и передадим туда новый метод с параметром "item". Но мы не можем это сделать напрямую, т.к. уже деструктурировали объект item и у нас выбор, либо избавится от деструктуризации и добавить везде, где нужно префикс "item.", либо обратиться к элементу массива через индекс items[index]. ↑ -->
      <li
        v-for="({ id, label, purchased, highPriority }, index) in items"
        @click="togglePurchased(items[index - 1])"
        :key="id"
        :class="{ strikeout: purchased, priority: highPriority }"
        class="some-static-class"
      >
        {{ ++index }}: {{ label }}
      </li>
      <!-- 7.4 Ещё одна возможность добавить классы это добавить их массивом. Теперь каждый элемент массива будет отдельным CSS-классом. Но, если мы захотим использовать условия в подобном синтаксисе, то нужно использовать тернарный оператор. А также мы можем добавить ещё и статический класс, если какое-то условие, например true (как в случае с purchased добавим ".text-gray") или false ('.underline'). -->
      <!-- <li
        v-for="({ id, label, purchased, highPriority }, index) in items"
        :key="id"
        class="some-static-class"
        :class="[purchased ? 'strikeout text-gray' : 'underline', highPriority ? 'priority' : '']"
      >
        {{ ++index }}: {{ label }}
      </li> -->
      <!-- 7.5 И наконец, мы можем объединять синтаксис объекта и массива. ↑ -->
      <!-- <li
        v-for="({ id, label, purchased, highPriority }, index) in items"
        :key="id"
        class="some-static-class"
        :class="[{ strikeout: purchased }, { priority: highPriority }]"
      >
        {{ ++index }}: {{ label }}
      </li> -->
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
ul li {
  user-select: none;
}
</style>
