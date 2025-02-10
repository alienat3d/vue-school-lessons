<!-- ? Развёрнутая статья о применении атрибута "key": https://vueschool.io/articles/vuejs-tutorials/tips-and-gotchas-for-using-key-with-v-for-in-vue-js-3/ -->
<script setup>
import { ref } from 'vue'

const header = ref('Shopping List App')
// 2.1 Создадим ещё одну переменную "items" для пунктов списка и сделаем её реактивной ссылкой.
const items = ref([
  { id: 1, label: '10 party hats' },
  { id: 2, label: '2 board games' },
  { id: 3, label: '20 cups' },
])
// 2.5 Кстати, мы можем также работать и с объектами вместо массивов. Однако для данной задачи он нам не слишком подойдёт, т.ч. оставим его закомментированным.
/* const items = ref({
  'item-1': { id: 1, label: '10 party hats' },
  'item-2': { id: 2, label: '2 board games' },
  'item-3': { id: 3, label: '20 cups' },
}) */
</script>

<template>
  <div>
    <h1>{{ header }}</h1>
    <!-- * 2.0 Vue позволяет легко и быстро рендерить объекты и массивы. К примеру у нас есть список, но мы не хотим его прописывать вручную. -->
    <ul>
      <!--       <li>10 party hats</li>
      <li>2 board games</li>
      <li>20 cups</li> -->
      <!-- 2.2 Теперь пропишем один li с циклом v-for и укажем значением items. И чтобы снять ошибку добавим уникальный ключ в массив "items", т.ч. теперь это уже будет массив объектов. -->
      <!-- ? 2.3 И хотя это не обязательное условие для простейших массивов, но чтобы продемонстрировать хорошие практики. -->
      <!-- <li v-for="item in items" :key="item.id">{{ item.label }}</li> -->
      <!-- 2.4 И, раз уж массив "items" содержит объекты, то мы можем деструктурировать "id" и "label", а также добавим индекс и пронумеруем наш список: ↑ -->
      <li v-for="({ id, label }, index) in items" :key="id">{{ ++index }}: {{ label }}</li>
      <!-- <li v-for="{ id, label } in items" :key="id">{{ ++index }}: {{ label }}</li> -->
    </ul>
  </div>
</template>

<style></style>
