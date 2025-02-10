<script setup>
import { reactive } from 'vue'

let state = reactive({ count: 0 })

const increment = () => {
  state = {
    count: state.count++,
  }
}
</script>

<template>
  <div>
    <p>{{ state.count }}</p>
    <button @click="increment" class="btn btn-primary">Increment Count</button>
  </div>
</template>

<style></style>
