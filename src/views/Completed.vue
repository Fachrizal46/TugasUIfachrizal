<template>
  <div class="completed-page">
    <h2>Completed ✔️</h2>

    <ul class="completed-list">
      <li v-for="item in completedTasks" :key="item.id" class="completed-item">
        {{ item.text }}
      </li>
    </ul>

    <!-- Jika tidak ada data -->
    <p v-if="completedTasks.length === 0" class="empty-text">
      Belum ada tugas yang selesai.
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todos: []
    }
  },

  created() {
    const saved = localStorage.getItem("todos")
    this.todos = saved ? JSON.parse(saved) : []
  },

  computed: {
    completedTasks() {
      return this.todos.filter(t => t.done)   // ← PERBAIKI DI SINI, DULUNYA t.completed
    }
  }
}
</script>

<style>
.completed-page {
  max-width: 420px;
  margin: auto;
  padding: 20px;
  font-family: Arial;
}

.completed-list {
  list-style: none;
  padding: 0;
}

.completed-item {
  background: #e8ffe8;
  padding: 12px;
  border-radius: 10px;
  margin-bottom: 10px;
  color: #065f46;
  font-weight: bold;
  box-shadow: 0 1px 4px #0000001a;
}

.empty-text {
  text-align: center;
  color: gray;
  margin-top: 20px;
}
</style>
