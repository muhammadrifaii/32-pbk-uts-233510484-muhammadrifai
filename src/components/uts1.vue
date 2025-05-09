<script setup>
import { ref, computed } from 'vue'

const newTask = ref('')
const tasks = ref([])
const filter = ref('all')

function addTask() {
  if (newTask.value.trim()) {
    tasks.value.push({ text: newTask.value, completed: false })
    newTask.value = ''
  }
}

function removeTask(index) {
  tasks.value.splice(index, 1)
}

const filteredTasks = computed(() => {
  if (filter.value === 'unfinished') {
    return tasks.value.filter(task => !task.completed)
  }
  return tasks.value
})

function filterBtnClass(type) {
  return filter.value === type ? 'active-filter' : ''
}

</script>

<template>
  <div class="todo-app">
    <div class="container">
      <h1>Agenda hari ini</h1>
      <div class="input-group">
        <input v-model="newTask" @keyup.enter="addTask" placeholder="Tambah kegiatan baru" />
        <button @click="addTask">Tambah</button>
      </div>
      <div class="filter-group">
        <button @click="filter = 'all'" :class="filterBtnClass('all')">Semua</button>
        <button @click="filter = 'unfinished'" :class="filterBtnClass('unfinished')">Belum Selesai</button>
      </div>
      <ul class="task-list">
        <li v-for="(task, index) in filteredTasks" :key="index">
          <div class="task-item">
            <input type="checkbox" v-model="task.completed" />
            <span :class="{ done: task.completed }">{{ task.text }}</span>
          </div>
          <button class="delete" @click="removeTask(index)">❌</button>
        </li>
      </ul>
      <p v-if="filteredTasks.length === 0" class="empty-msg">
        Tidak ada kegiatan untuk ditampilkan.
      </p>
    </div>
  </div>
</template>


<style scoped>

.todo-app {
  min-height: 100vh;
  background: #f3f4f6;
  padding: 2rem;
  font-family: sans-serif;
}
.container {
  max-width: 600px;
  margin: auto;
  background: white;
  padding: 2rem;
  border-radius: 1rem;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  font-size: 2.25rem;
  font-weight: 700;
  color: #111827;
  margin-bottom: 1.5rem;
}
.input-group {
  display: flex;
  margin-bottom: 1rem;
}
.input-group input {
  flex-grow: 1;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-top-left-radius: 0.5rem;
  border-bottom-left-radius: 0.5rem;
  outline: none;
}
.input-group button {
  background: #3b82f6;
  color: white;
  padding: 0 1rem;
  border: none;
  border-top-right-radius: 0.5rem;
  border-bottom-right-radius: 0.5rem;
  cursor: pointer;
}
.input-group button:hover {
  background: #2563eb;
}
.filter-group {
  display: flex;
  justify-content: flex-end;
  gap: 0.5rem;
  margin-bottom: 1rem;
}
.filter-group button {
  padding: 0.3rem 0.75rem;
  border-radius: 9999px;
  border: none;
  background: #e5e7eb;
  cursor: pointer;
}
.filter-group button:hover {
  background: #d1d5db;
}
.filter-group .active-filter {
  background: #3b82f6;
  color: white;
}
.task-list {
  list-style: none;
  padding: 0;
}
.task-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}
.task-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.5rem 0;
  border-bottom: 1px solid #e5e7eb;
}
.task-list li .delete {
  background: transparent;
  border: none;
  color: #ef4444;
  font-size: 1.2rem;
  cursor: pointer;
}
.task-list li .delete:hover {
  color: #b91c1c;
}
.done {
  text-decoration: line-through;
  color: #9ca3af;
}
.empty-msg {
  text-align: center;
  color: #6b7280;
  margin-top: 1rem;
}

</style>