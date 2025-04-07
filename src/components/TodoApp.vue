<template>
  <div :class="['min-h-screen', darkMode ? 'dark bg-gray-900 text-white' : 'bg-gray-100 text-gray-900']">
    <div class="flex justify-end p-4">
      <button @click="toggleDarkMode" class="text-sm px-3 py-1 rounded bg-indigo-500 text-white hover:bg-indigo-600 transition">
        {{ darkMode ? 'Light Mode ☀️' : 'Dark Mode 🌙' }}
      </button>
    </div>

    <div class="max-w-md mx-auto bg-white dark:bg-gray-800 shadow-lg rounded-lg p-6">
      <h1 class="text-2xl font-bold text-center mb-4">Vue Todo App</h1>

      <!-- Input -->
      <div class="flex items-center gap-2 mb-4">
        <input
          v-model="newTodo"
          @keyup.enter="addTodo"
          placeholder="Add a task..."
          class="flex-grow px-4 py-2 border rounded dark:bg-gray-700 dark:text-white dark:border-gray-600"
        />
        <button @click="addTodo" class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 transition">Add</button>
      </div>

      <!-- Filters -->
      <div class="flex justify-around mb-4 text-sm font-semibold">
        <button v-for="f in ['all', 'active', 'completed']" :key="f"
          @click="filter = f"
          :class="[
            'px-3 py-1 rounded',
            filter === f ? 'bg-blue-500 text-white' : 'bg-gray-200 dark:bg-gray-600 dark:text-white'
          ]">
          {{ f.charAt(0).toUpperCase() + f.slice(1) }}
        </button>
      </div>

      <!-- Todo List -->
      <transition-group name="fade-slide" tag="ul" class="space-y-2">
        <li
          v-for="(todo, index) in filteredTodos"
          :key="todo.text + index"
          class="flex justify-between items-center px-4 py-2 bg-gray-100 dark:bg-gray-700 rounded shadow-sm"
        >
          <label class="flex items-center gap-2 cursor-pointer">
            <input type="checkbox" v-model="todo.completed" />
            <span :class="{ 'line-through opacity-60': todo.completed }">{{ todo.text }}</span>
          </label>
          <button @click="removeTodo(index)" class="text-red-500 hover:text-red-700 transition">✖</button>
        </li>
      </transition-group>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const newTodo = ref('');
const todos = ref([]);
const filter = ref('all');
const darkMode = ref(false);

const addTodo = () => {
  if (newTodo.value.trim()) {
    todos.value.push({ text: newTodo.value.trim(), completed: false });
    newTodo.value = '';
  }
};

const removeTodo = (index) => {
  todos.value.splice(index, 1);
};

const toggleDarkMode = () => {
  darkMode.value = !darkMode.value;
};

const filteredTodos = computed(() => {
  if (filter.value === 'active') return todos.value.filter(todo => !todo.completed);
  if (filter.value === 'completed') return todos.value.filter(todo => todo.completed);
  return todos.value;
});
</script>

<style scoped>
.fade-slide-enter-active,
.fade-slide-leave-active {
  transition: all 0.4s ease;
}
.fade-slide-enter-from {
  opacity: 0;
  transform: translateY(-10px);
}
.fade-slide-leave-to {
  opacity: 0;
  transform: translateX(20px);
}
</style>
