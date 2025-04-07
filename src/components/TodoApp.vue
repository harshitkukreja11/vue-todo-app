<template>
  <div class="app" :class="{ dark: darkMode }">
    <div class="todo-container">
      <div class="header">
        <h1>Vue Todo App</h1>
        <label class="dark-toggle">
          <input type="checkbox" v-model="darkMode" />
          🌙 Dark Mode
        </label>
      </div>

      <form @submit.prevent="addTodo">
        <input
          v-model="newTodo"
          type="text"
          placeholder="What do you need to do?"
        />
        <button type="submit">Add</button>
      </form>

      <div class="filters">
        <button :class="{ active: filter === 'all' }" @click="filter = 'all'">
          All
        </button>
        <button :class="{ active: filter === 'active' }" @click="filter = 'active'">
          Active
        </button>
        <button :class="{ active: filter === 'completed' }" @click="filter = 'completed'">
          Completed
        </button>
      </div>

      <transition-group name="fade" tag="ul" class="todo-list">
        <li
          v-for="(todo, index) in filteredTodos"
          :key="todo.text + index"
          :class="{ done: todo.done }"
        >
          <span>{{ todo.text }}</span>
          <div class="actions">
            <button @click="toggleTodo(index)">✔</button>
            <button @click="removeTodo(index)">✖</button>
          </div>
        </li>
      </transition-group>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [],
      filter: 'all',
      darkMode: false,
    };
  },
  computed: {
    filteredTodos() {
      if (this.filter === 'active') return this.todos.filter(t => !t.done);
      if (this.filter === 'completed') return this.todos.filter(t => t.done);
      return this.todos;
    },
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim()) {
        this.todos.push({ text: this.newTodo.trim(), done: false });
        this.newTodo = '';
      }
    },
    toggleTodo(index) {
      this.todos[index].done = !this.todos[index].done;
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
  },
};
</script>

<style scoped>
/* Base styles */
.app {
  min-height: 100vh;
  background: linear-gradient(to right, #d3cce3, #e9e4f0);
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 2rem;
  transition: 0.3s ease;
}

.todo-container {
  background-color: white;
  padding: 2rem;
  border-radius: 15px;
  width: 100%;
  max-width: 450px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
  transition: 0.3s ease;
}

/* Header with dark toggle */
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}
h1 {
  margin: 0;
  color: #4a4e69;
}
.dark-toggle {
  font-size: 0.9rem;
}

/* Form */
form {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1rem;
}
input[type='text'] {
  flex: 1;
  padding: 0.6rem;
  border: 1px solid #ccc;
  border-radius: 8px;
  font-size: 1rem;
}
button[type='submit'] {
  padding: 0.6rem 1rem;
  background-color: #4a4e69;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: 0.3s ease;
}
button[type='submit']:hover {
  background-color: #22223b;
}

/* Filters */
.filters {
  display: flex;
  justify-content: space-around;
  margin: 1rem 0;
}
.filters button {
  background: none;
  border: none;
  font-weight: bold;
  cursor: pointer;
  color: #4a4e69;
}
.filters button.active {
  color: #22223b;
  text-decoration: underline;
}

/* Todo list */
.todo-list {
  list-style: none;
  padding: 0;
  margin: 0;
}
.todo-list li {
  background-color: #f4f4f4;
  padding: 0.75rem;
  border-radius: 8px;
  margin-bottom: 0.5rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: 0.3s ease;
}
.todo-list li.done span {
  text-decoration: line-through;
  color: gray;
}

/* Action buttons */
.actions button {
  margin-left: 0.5rem;
  background-color: transparent;
  color: #4a4e69;
  font-size: 1.2rem;
  border: none;
  cursor: pointer;
}
.actions button:hover {
  color: #22223b;
}

/* Animations */
.fade-enter-active,
.fade-leave-active {
  transition: all 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(10px);
}

/* Dark mode */
.dark {
  background: #1e1e2e;
  color: white;
}
.dark .todo-container {
  background: #2e2e3e;
}
.dark input,
.dark button {
  background-color: #444;
  color: white;
}
.dark .filters button {
  color: #ddd;
}
.dark .filters button.active {
  color: white;
}
.dark .todo-list li {
  background-color: #3a3a4d;
}
</style>
