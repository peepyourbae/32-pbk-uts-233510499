<script setup>
import { ref, computed } from 'vue';

const items = ref([])
const newItem = ref('')
const filtered = ref("all")

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

const deleteItem = (id) => {
  items.value = items.value.filter(item => item.id !== id)
}

const toggle = (item) => {
  item.completed = !item.completed
  console.log(item.completed)
}

const filteredItems = computed(() => {
  if (filtered.value === 'completed') {
    return items.value.filter(i => i.completed)
  }
  if (filtered.value === 'incomplete') {
    return items.value.filter(i => !i.completed)
  }
  return items.value
})

</script>

<template>
  <h1>To-Do List</h1>
  <input v-model="newItem" type="text" @keyup.enter="addItem" placeholder="Add a new item">
  <button @click="addItem">Add</button>
  <select v-model="filtered">
    <option value="all">Semua</option>
    <option value="completed">Selesai</option>
    <option value="incomplete">Belum Selesai</option>
  </select>

  <div>
    <ul>
      <li v-for="item in filteredItems" :key="item.id">
        <input type="checkbox" @change="toggle(item)" :checked="item.completed" />
        {{ item.text }}
        <button @click="deleteItem(item.id)">Hapus</button>
      </li>
    </ul>
  </div>
</template>

<style scoped></style>
