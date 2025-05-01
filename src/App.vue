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
  <div class="w-screen h-screen bg-gradient-to-br from-purple-100 via-white to-blue-100 flex items-center justify-center p-4">
    <div class="w-full max-w-xl h-full bg-white shadow-xl rounded-2xl flex flex-col p-6">
      <h1 class="text-3xl font-bold mb-4 text-center text-purple-700">To-Do List</h1>

      <div class="flex gap-2 mb-4">
        <input 
          v-model="newItem"
          type="text"
          @keyup.enter="addItem"
          placeholder="Tambahkan item baru..."
          class="flex-1 p-2 bg-gray-100 border rounded focus:outline-none focus:ring-2 focus:ring-purple-400"
        />
        <button
          @click="addItem"
          class="px-4 py-2 bg-purple-600 text-white rounded hover:bg-purple-700"
        >
          Add
        </button>
      </div>

      <div class="mb-4">
        <select
          v-model="filtered"
          class="w-full p-2 bg-gray-100 border rounded focus:outline-none focus:ring-2 focus:ring-purple-400"
        >
          <option value="all">Semua</option>
          <option value="completed">Selesai</option>
          <option value="incomplete">Belum Selesai</option>
        </select>
      </div>

      <div class="flex-1 overflow-y-auto border rounded p-2 bg-gray-50">
        <ul class="space-y-2">
          <li
            v-for="item in filteredItems"
            :key="item.id"
            class="flex items-center justify-between p-2 bg-white border rounded hover:bg-purple-50 transition"
          >
            <div class="flex items-center gap-2">
              <input
                type="checkbox"
                @change="toggle(item)"
                :checked="item.completed"
                class="w-4 h-4 text-purple-600"
              />
              <span :class="{ 'line-through text-gray-500': item.completed }">
                {{ item.text }}
              </span>
            </div>
            <button
              @click="deleteItem(item.id)"
              class="text-red-500 hover:text-red-700 text-sm"
            >
              Hapus
            </button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
