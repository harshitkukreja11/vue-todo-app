<template>
  <div class="todo-container" :class="{ dark: darkMode }">
    <div class="header">
      <h1>Vue Todo App</h1>
      <label>
        <input type="checkbox" v-model="darkMode" /> 🌙 Dark Mode
      </label>
    </div>

    <form @submit.prevent="addTodo">
      <input v-model="newTodo" placeholder="Enter a task..." />
      <button>Add</button>
    </form>

    <div class="filters">
      <button :class="{ active: filter === 'all' }" @click="filter = 'all'">All</button>
      <button :class="{ active: filter === 'active' }" @click="filter = 'active'">Active</button>
      <button :class="{ active: filter === 'completed' }" @click="filter = 'completed'">Completed</button>
    </div>

    <transition-group tag="ul" class="todo-list" name="fade">
      <li
        v-for="(todo, index) in filteredTodos"
        :key="todo.id"
        :class="{ done: todo.done }"
        draggable="true"
        @dragstart="startDrag(index)"
        @dragover.prevent
        @drop="dropTodo(index)"
      >
        <div class="text">
          <span v-if="!todo.editing" @dblclick="editTodo(todo)">
            {{ todo.text }}
          </span>
          <input
            v-else
            v-model="todo.text"
            @keyup.enter="doneEditing(todo)"
            @blur="doneEditing(todo)"
          />
          <small class="timestamp">{{ formatTime(todo.time) }}</small>
        </div>

        <div class="actions">
          <button @click="toggleTodo(index)">✔</button>
          <button @click="removeTodo(index)">✖</button>
        </div>
      </li>
    </transition-group>
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
      draggedIndex: null,
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
      if (!this.newTodo.trim()) return;
      this.todos.push({
        id: Date.now(),
        text: this.newTodo.trim(),
        done: false,
        editing: false,
        time: new Date(),
      });
      this.newTodo = '';
    },
    toggleTodo(index) {
      this.todos[index].done = !this.todos[index].done;
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    editTodo(todo) {
      todo.editing = true;
      this.$nextTick(() => {
        const inputs = this.$el.querySelectorAll('input');
        inputs.forEach(input => input.focus());
      });
    },
    doneEditing(todo) {
      todo.editing = false;
    },
    formatTime(date) {
      const d = new Date(date);
      return `${d.toLocaleDateString()} ${d.toLocaleTimeString()}`;
    },
    startDrag(index) {
      this.draggedIndex = index;
    },
    dropTodo(index) {
      const draggedTodo = this.todos[this.draggedIndex];
      this.todos.splice(this.draggedIndex, 1);
      this.todos.splice(index, 0, draggedTodo);
    },
  },
};
</script>

<style scoped>
.todo-container {
  max-width: 480px;
  margin: 3rem auto;
  padding: 2rem;
  border-radius: 16px;
  background: linear-gradient(135deg, #f0f0f3, #ffffff);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
  transition: background 0.3s, color 0.3s;
}
.dark {
  background: linear-gradient(135deg, #1e1e2f, #2e2e3e);
  color: #f4f4f4;
}
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}
form {
  display: flex;
  margin-bottom: 1rem;
}
input {
  flex: 1;
  padding: 0.6rem 0.8rem;
  border-radius: 8px;
  border: 1px solid #ccc;
  outline: none;
}
button {
  margin-left: 0.5rem;
  padding: 0.6rem 1rem;
  background: #4a4e69;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: 0.2s;
}
button:hover {
  background: #373a50;
}
.filters {
  display: flex;
  justify-content: space-around;
  margin-bottom: 1rem;
}
.filters button {
  background-color: #e0e0e0;
  border: none;
  padding: 0.4rem 0.9rem;
  border-radius: 20px;
  font-weight: bold;
  color: #333;
  cursor: pointer;
  transition: background 0.3s, transform 0.2s;
}
.filters button:hover {
  background-color: #c6c6f5;
  transform: scale(1.05);
}
.filters .active {
  background-color: #4a4e69;
  color: #fff;
}
.todo-list {
  list-style: none;
  padding: 0;
}
.todo-list li {
  background: #fafafa;
  padding: 0.8rem;
  border-radius: 10px;
  margin-bottom: 0.6rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: background 0.2s;
}
.dark .todo-list li {
  background: #393b4b;
}
.todo-list li.done span {
  text-decoration: line-through;
  color: #888;
}
.todo-list li input[type="text"] {
  padding: 0.5rem;
  border-radius: 6px;
  border: 1px solid #ccc;
  background-color: #fff;
  outline: none;
}
.actions button {
  background: none;
  border: none;
  font-size: 1rem;
  margin-left: 0.5rem;
  cursor: pointer;
  color: #444;
  transition: color 0.2s;
}
.actions button:hover {
  color: #000;
}
.timestamp {
  display: block;
  font-size: 0.75rem;
  color: #999;
  margin-top: 4px;
}
.fade-enter-active,
.fade-leave-active {
  transition: all 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(10px);
}
</style>

