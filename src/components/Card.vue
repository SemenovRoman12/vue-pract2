<script>
export default {
  name: "Card",
  props: {
    title: String,
    notes: Array,
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
        this.$emit("move-to-done");
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
  <div class="card">
    <h3>{{ title }}</h3>
    <ul>
      <li v-for="(task, index) in notes" :key="index">
        <input type="checkbox" v-model="task.done" />
        {{ task.text }}
      </li>
    </ul>
  </div>
</template>

<style>

</style>
