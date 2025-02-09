<script setup>
import { ref } from 'vue'

const msg = ref('Hello World!')
</script>

<template>
  <div>
    <h1>Getting Started with Vue.js and the Composition API</h1>
    <!-- * 1.0 Данные между инпутами и выводом в <p> полностью синхронизированы "в оба конца" с помощью директивы "v-model". -->
    <!-- 1.1 Мы также можем использовать в фигурных скобках JS-методы, будто это обычный JS-файл. Например сделаем вывод заглавными буквами методом "toUpperCase()". Но есть ограничение, в подобной записи мы можем использовать лишь 1 строчку кода. А также внутри {{  }} мы не можем создавать новые переменные или указывать условия (однако тернарный оператор всё же работает). -->
    <!-- <p>{{ msg.toUpperCase() }}</p> -->
    <!-- 1.2 Или можно использовать условные операторы вместо этого. -->
    <!-- Go to [src\views\02ShoppingListAppView.vue] -->
    <p>{{ msg || 'Hi there!' }}</p>
    <input v-model="msg" />
    <input v-model="msg" />
  </div>
</template>

<style></style>
