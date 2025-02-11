<script>
import Card from "@/components/Card.vue";

export default {
  name: "Columns",
  components: { Card },
  data() {
    return {
      columns: {
        todo: [],
        inProgress: [],
        done: []
      },
      maxTodo: 3,
      maxInProgress: 5,
      isTodoLocked: false
    };
  },
  methods: {
    addCard(newCard) {
      if (this.columns.todo.length < this.maxTodo) {
        this.columns.todo.push(newCard);
        this.sortTasks("todo");
      } else {
        alert("Максимум 3 карточки в первой колонке!");
      }
    },
    moveToInProgress(index) {
      if (this.columns.inProgress.length < this.maxInProgress) {
        const card = this.columns.todo.splice(index, 1)[0];
        this.columns.inProgress.push(card);
        this.sortTasks("inProgress");
      }
      this.checkLocks();
    },
    moveBackToTodo(index) {
      const card = this.columns.inProgress.splice(index, 1)[0];
      if (this.columns.todo.length < this.maxTodo) {
        this.columns.todo.push(card);
        this.sortTasks("todo");
      } else {
        alert("Нет места в первой колонке!");
        this.columns.inProgress.push(card);
      }
      this.checkLocks();
    },
    moveToDone(index, completedAt) {
      const card = this.columns.inProgress.splice(index, 1)[0];
      card.completedAt = completedAt;
      this.columns.done.push(card);
      this.sortTasks("done");
    },
    sortTasks(columnName) {
      if (columnName === "done") {
        this.columns.done.sort(
            (a, b) => new Date(a.completedAt) - new Date(b.completedAt)
        );
      } else {
        this.columns[columnName].sort((a, b) => a.priority - b.priority);
      }
    },
    checkLocks() {
      this.isTodoLocked = this.columns.inProgress.length >= this.maxInProgress;
    }
  }
};
</script>

<template>
  <div class="wrapper">
    <div class="column column_todo">
      <h2>To Do</h2>
      <Card
          v-for="(card, index) in columns.todo"
          :key="index"
          :title="card.title"
          :notes="card.notes"
          :priority="card.priority"
          @move-to-inProgress="moveToInProgress(index)"
      />
    </div>
    <div class="column column_in-progress">
      <h2>In Progress</h2>
      <Card
          v-for="(card, index) in columns.inProgress"
          :key="index"
          :title="card.title"
          :notes="card.notes"
          :priority="card.priority"
          @move-to-done="moveToDone(index, $event)"
          @move-back-to-todo="moveBackToTodo(index)"
      />
    </div>
    <div class="column column_done">
      <h2>Done</h2>
      <Card
          v-for="(card, index) in columns.done"
          :key="index"
          :title="card.title"
          :notes="card.notes"
          :priority="card.priority"
          :completedAt="card.completedAt"
      />
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  display: flex;
  justify-content: space-between;
  padding: 20px;
  gap: 20px;
}

.column {
  width: 30%;
  background: #f8f9fa;
  border-radius: 8px;
  padding: 15px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
  min-height: 300px;
}

h2 {
  text-align: center;
  color: #444;
  margin-bottom: 15px;
}

.column_todo {
  border-left: 5px solid #007bff;
}

.column_in-progress {
  border-left: 5px solid #ffc107;
}

.column_done {
  border-left: 5px solid #28a745;
}
</style>
