<template>
  <div class="todo-container">

    <!-- PROGRESS BAR -->
    <div class="progress-card">
      <p class="progress-text">
        Progress: {{ completedCount }} / {{ todos.length }}
      </p>

      <div class="progress-bar">
        <div class="progress-fill" :style="{ width: progressPercentage + '%' }"></div>
      </div>
    </div>

    <!-- INPUT -->
    <div class="add-task">
      <input 
        v-model="newTask" 
        @keyup.enter="addTask"
        placeholder="Tambah tugas baru..." 
      />
      <button @click="addTask">Add</button>
    </div>

    <!-- LIST -->
    <ul class="todo-list">
      <li v-for="todo in todos" :key="todo.id" class="todo-item">

        <label class="checkbox-wrapper">
          <input type="checkbox" v-model="todo.done" @change="saveToLocalStorage" />
          <span :class="{ done: todo.done }">{{ todo.text }}</span>
        </label>

        <button class="delete-btn" @click="remove(todo.id)">✖</button>
      </li>
    </ul>

  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      todos: []
    }
  },

  created() {
    const saved = localStorage.getItem("todos")
    this.todos = saved ? JSON.parse(saved) : []
  },

  computed: {
    completedCount() {
      return this.todos.filter(t => t.done).length
    },
    progressPercentage() {
      if (this.todos.length === 0) return 0
      return (this.completedCount / this.todos.length) * 100
    }
  },

  methods: {
    addTask() {
      if (!this.newTask.trim()) return

      this.todos.push({
        id: Date.now(),
        text: this.newTask,
        done: false
      })

      this.newTask = ""
      this.saveToLocalStorage()
    },

    remove(id) {
      this.todos = this.todos.filter(t => t.id !== id)
      this.saveToLocalStorage()
    },

    saveToLocalStorage() {
      localStorage.setItem("todos", JSON.stringify(this.todos))
    }
  }
}
</script>

<style>

.todo-container {
  max-width: 420px;
  margin: auto;
  padding: 20px;
  font-family: Arial;
}

.progress-card {
  background: #f1f5f9;
  padding: 15px;
  border-radius: 12px;
  margin-bottom: 20px;
  box-shadow: 0 2px 5px #0000001a;
}

.progress-text {
  margin-bottom: 6px;
  font-weight: bold;
}

.progress-bar {
  width: 100%;
  height: 10px;
  background: #d1d5db;
  border-radius: 6px;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: #10b981;
  transition: width 0.3s;
}

.add-task {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.add-task input {
  flex: 1;
  padding: 8px 10px;
  border-radius: 8px;
  border: 1px solid #d1d5db;
}

.add-task button {
  background: #3b82f6;
  border: none;
  padding: 8px 15px;
  color: white;
  border-radius: 8px;
  cursor: pointer;
}

.todo-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.todo-item {
  background: #ffffff;
  padding: 12px;
  margin-bottom: 8px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-radius: 10px;
  box-shadow: 0 1px 4px #0000001a;
}

.checkbox-wrapper {
  display: flex;
  align-items: center;
  gap: 10px;
}

.done {
  text-decoration: line-through;
  color: gray;
}

.delete-btn {
  background: transparent;
  border: none;
  color: #ef4444;
  font-size: 18px;
  cursor: pointer;
}

</style>
