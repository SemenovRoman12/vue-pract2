<script>
export default {
  name: "Card",
  props: {
    title: String,
    notes: Array,
    completedAt: String,
  },
  computed: {
    completedCount() {
      return this.notes.filter(note => note.done).length;
    },
    isCompleted() {
      return this.completedCount === this.notes.length;
    },
    isInProgress() {
      return this.completedCount / this.notes.length >= 0.5;
    }
  },
  watch: {
    isCompleted(newVal) {
      if (newVal) {
        this.$emit("move-to-done", new Date().toLocaleString());
      }
    },
    isInProgress(newVal, oldVal) {
      if (newVal && !oldVal) {
        this.$emit("move-to-inProgress");
      } else if (!newVal && oldVal) {
        this.$emit("move-back-to-todo");
      }
    }
  }
};
</script>

<template>
  <div class="card" :class="{ 'completed-card': isCompleted }">
    <h3>{{ title }}</h3>
    <ul>
      <li v-for="(task, index) in notes" :key="index">
        <input type="checkbox" v-model="task.done" :disabled="isCompleted" />
        {{ task.text }}
      </li>
    </ul>
    <p v-if="isCompleted" class="completed-date">Завершено: {{ completedAt }}</p>
  </div>
</template>

<style scoped>
.card {
  background: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
  padding: 15px;
  margin: 10px 0;
  transition: transform 0.2s ease-in-out;
}

.card:hover {
  transform: scale(1.02);
}

.completed-card {
  background: #e6ffe6;
  border: 2px solid #28a745;
}

h3 {
  margin: 0 0 10px;
  color: #333;
}

ul {
  padding: 0;
  list-style: none;
}

li {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 5px;
}

input[type="checkbox"] {
  width: 18px;
  height: 18px;
  cursor: pointer;
}

.completed-date {
  font-size: 14px;
  color: #28a745;
  font-weight: bold;
  text-align: center;
  margin-top: 10px;
}
</style>
