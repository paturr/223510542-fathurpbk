<template>
  <div class="todo-app">
    <h1>Kegiatan Sehari-hari</h1>
    <p>Created by Fathur Azrahman</p>

    <div class="filter-buttons">
      <button @click="filter = 'all'" :class="{ active: filter === 'all' }">Semua</button>
      <button @click="filter = 'done'" :class="{ active: filter === 'done' }">Selesai</button>
      <button @click="filter = 'pending'" :class="{ active: filter === 'pending' }">Belum Selesai</button>
    </div>

    <input type="text" v-model="newTodo" placeholder="Masukkan item kegiatan baru" @keyup.enter="addTodo" />
    <button @click="addTodo">Tambah</button>
    <button @click="removeAllTodos" :disabled="todos.length === 0">Hapus Semua</button>
    <hr>
    <ul>
      <li v-for="(todo, index) in filteredTodos" :key="index" :class="{ done: todo.done }">
        <input type="checkbox" v-model="todo.done" />
        <input v-if="todo.editMode" type="text" v-model="todo.text" @blur="updateTodoText(index)" @keyup.enter="updateTodoText(index)" />
        <span v-else>{{ todo.text }}</span>
        <button @click="toggleEditMode(todo)">Edit</button>
        <button @click="removeTodo(index)">Hapus</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [],
      filter: 'all', // 'all', 'done', 'pending'
    };
  },
  computed: {
    filteredTodos() {
      if (this.filter === 'all') {
        return this.todos;
      } else if (this.filter === 'done') {
        return this.todos.filter(todo => todo.done);
      } else if (this.filter === 'pending') {
        return this.todos.filter(todo => !todo.done);
      }
    },
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim()) {
        this.todos.push({
          text: this.newTodo.trim(),
          done: false,
          editMode: false // Tambahkan editMode untuk setiap item
        });
        this.newTodo = '';
      }
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    removeAllTodos() {
      this.todos = [];
    },
    updateTodoText(index) {
      if (index >= 0 && index < this.todos.length) {
        this.todos[index].text = this.todos[index].text.trim();
        this.todos[index].editMode = false; // Nonaktifkan mode pengeditan setelah mengubah teks
      }
    },
    toggleEditMode(todo) {
      todo.editMode = !todo.editMode; // Toggle mode pengeditan untuk item yang dipilih
    },
  },
};
</script>

<style scoped>
.todo-app {
  font-family: sans-serif;
}

ul > li > button {
  background-color: red;
  color: white;
}

.todo-app input[type="text"] {
  padding: 10px;
  margin-bottom: 10px;
}

.todo-app button {
  padding: 5px 10px;
  cursor: pointer;
}

.todo-app button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.todo-app ul {
  list-style: none;
  padding: 0;
}

.todo-app li {
  margin-bottom: 10px;
}

.todo-app li input[type="checkbox"] {
  margin-right: 10px;
}

.todo-app li span {
  text-decoration: none;
}

.todo-app li button {
  float: right;
  padding: 5px;
  border: none;
  cursor: pointer;
}

.todo-app li.done span {
  text-decoration: line-through;
}

.filter-buttons {
  margin-bottom: 10px;
}

.filter-buttons button {
  padding: 5px 10px;
  margin-right: 5px;
  cursor: pointer;
  border: 1px solid #ccc;
}

.filter-buttons button.active {
  background-color: #eee;
}
</style>
