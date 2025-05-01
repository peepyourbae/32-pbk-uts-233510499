<script setup>
import { ref } from 'vue';

const items = ref([])
const newItem = ref('')

const addItem = () => {
  const text = newItem.value.trim()
  if (text) {
    items.value.push({
      id: Date.now(),
      text,
      completed: false
    })
    newItem.value = ''
  }
}
</script>

<template>
  <h1>To-Do List</h1>
  <input v-model="newItem" type="text" @keyup.enter="addItem" placeholder="Add a new item">
  <button @click="addItem">Add</button>


  <div>
    <ul>
      <li v-for="item in items" :key="item.id">
        {{ item.text }}
      </li>
    </ul>
  </div>
</template>

<style scoped>

</style>
