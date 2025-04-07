<template>
  <div>
    <h1>Vue CLI Todo App</h1>
    <input v-model="newTodo" @keyup.enter="addTodo" placeholder="Add a task" />
    <button @click="addTodo">Add</button>

    <ul>
      <li v-for="(todo, index) in todos" :key="index">
        {{ todo }}
        <button @click="removeTodo(index)">Remove</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const newTodo = ref('');
const todos = ref([]);

const addTodo = () => {
  if (newTodo.value.trim()) {
    todos.value.push(newTodo.value.trim());
    newTodo.value = '';
  }
};

const removeTodo = (index) => {
  todos.value.splice(index, 1);
};
</script>

<style scoped>
input {
  margin-right: 10px;
}
li {
  margin: 8px 0;
}
</style>
