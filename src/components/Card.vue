<script>
export default {
  name: "Card",
  props: {
    title: String,
    notes: Array, // Основные задачи с их подпунктами
    priority: Number, // Приоритет задачи
    completedAt: String // Дата завершения
  },
  computed: {
    completedCount() {
      return this.notes.filter(note => note.done).length;
    },
    isCompleted() {
      return this.notes.every(note => note.done && this.areSubnotesCompleted(note));
    },
    isInProgress() {
      return this.completedCount / this.notes.length >= 0.5;
    },
    priorityLabel() {
      switch (this.priority) {
        case 1:
          return "Высокий";
        case 2:
          return "Средний";
        case 3:
          return "Низкий";
        default:
          return "";
      }
    }
  },
  methods: {
    areSubnotesCompleted(note) {
      return note.subnotes.every(subnote => subnote.done);
    },
    toggleMainTask(note) {
      if (this.areSubnotesCompleted(note)) {
        note.done = !note.done;
      } else {
        alert("Сначала выполните все подпункты этого пункта!");
      }
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
    <!-- Отображение приоритета -->
    <p class="priority">Приоритет: {{ priorityLabel }}</p>
    <ul class="tasks-list">
      <li v-for="(note, index) in notes" :key="index" class="task-item">
        <div class="main-task">
          <input
              type="checkbox"
              :checked="note.done"
              @change="toggleMainTask(note)"
              :disabled="!areSubnotesCompleted(note) || isCompleted"
          />
          {{ note.text }}
        </div>
        <!-- Подпункты -->
        <ul v-if="note.subnotes.length" class="subtasks-list">
          <li v-for="(subnote, subIndex) in note.subnotes" :key="subIndex" class="subtask-item">
            <input type="checkbox" v-model="subnote.done" />
            {{ subnote.text }}
          </li>
        </ul>
      </li>
    </ul>
    <!-- Дата завершения -->
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

.priority {
  font-size: 14px;
  color: #666;
  margin-bottom: 10px;
}

.tasks-list {
  padding: 0;
  list-style: none;
}

.task-item {
  margin-bottom: 10px;
}

.main-task {
  display: flex;
  align-items: center;
  gap: 10px;
}

.subtasks-list {
  padding-left: 20px; /* Табуляция для подпунктов */
  margin-top: 5px;
  list-style-type: none;
}

.subtask-item {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 14px;
}

.completed-date {
  font-size: 14px;
  color: #28a745;
  font-weight: bold;
  text-align: center;
  margin-top: 10px;
}
</style>
