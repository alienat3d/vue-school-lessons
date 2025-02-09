<script setup>
import { ref } from 'vue'

const header = ref('Shopping List App')
const items = ref([
  { id: 1, label: '10 party hats' },
  { id: 2, label: '2 board games' },
  { id: 3, label: '20 cups' },
])
// 3.1 Также нужно добавить ещё одну переменную с реактивными данными.
const newItem = ref('')
// const newItemPriority = ref('low')
// 3.5.1 Переименуем в newItemHighPriority и присвоим булево значение. ↓
const newItemHighPriority = ref(false)
// 3.6.0 Иногда чекбоксы используются также для сбора множественных ответов на один вопрос, создадим для этого новую переменную "iceCreamFlavors" и поместим в неё пустой массив. ↓
const iceCreamFlavors = ref([])
</script>

<!-- * 3.0 Добавим строку ввода, которая будет добавлять пункты в список. И чтобы привязать реактивные данные к строке ввода есть очень удобная "v-model" директива. Также можно сделать отображение менее "реактивным" и отображать только, когда с инпута пропадёт фокус с помощью модификатора "lazy". (есть и другие модификаторы, например "number" или "trim") -->
<!-- ? 3.1 Помимо строк ввода директиву "v-model" можно использовать также на textarea, select, чекбоксах, радио кнопках и др. -->
<!-- 3.2 Посмотрим как "v-model" работает на радио кнопках. Помимо установки лейбла для новой записи сделаем опцию установить приоритетность. Для этого создадим ещё одну реактивную переменную "newItemPriority" и по умолчанию значение будет "low". ↑ -->
<!-- 3.3 Теперь эти радио кнопки связаны между собой одной реактивной переменной "newItemPriority". -->
<template>
  <div>
    <h1>{{ header }}</h1>
    <!-- <p>{{ newItem }}</p> -->
    <input v-model.trim="newItem" type="text" placeholder="Add an item" />
    <!-- Priority:
    <label>
      <input v-model="newItemPriority" type="radio" value="low" />
      Low
    </label>
    <label>
      <input v-model="newItemPriority" type="radio" value="high" />
      High
    </label> -->
    <!-- 3.4 Похожим образом на радио кнопки работает и с элементом <select>. -->
    <!-- 3.5.0 Ну, а т.к. новый элемент выбора приоритетности имеет лишь два значения, то можно упростить логику до булево значения. ↑ -->
    <!-- <label>
      Priority:
      <select v-model="newItemHighPriority">
        <option value="false">Low</option>
        <option value="true">High</option>
      </select>
    </label> -->
    <!-- 3.5.2 ↑ Но теперь мы понимаем, что гораздо лучше для переключателя булево значения подходит чекбокс. -->
    <label>
      <input v-model="newItemHighPriority" type="checkbox" />
      High Priority
    </label>
    <!-- 3.6.1 А в темплейте будет 3 чекбокса для разных видом мороженого. -->
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
