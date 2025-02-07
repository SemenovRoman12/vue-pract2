<script>
export default {
  name: "CardForm",
  data() {
    return {
      title: "", // Заголовок задачи
      tasks: ["", "", ""], // Минимум 3 задачи
    };
  },
  computed: {
    isValid() {
      return (
          this.title.trim() !== "" &&
          this.tasks.slice(0, 3).every(task => task.trim() !== "")
      );
    }
  },
  methods: {
    addCard() {
      if (this.isValid) {
        this.$emit("add-card", {
          title: this.title,
          notes: this.tasks.map(task => ({ text: task, done: false })),
        });
        this.title = "";
        this.tasks = ["", "", ""]; // Очистка после добавления
      } else {
        alert("Введите заголовок и минимум 3 задачи!");
      }
    },
    addTask() {
      if (this.tasks.length < 5) {
        this.tasks.push("");
      }
    },
    removeTask(index) {
      if (this.tasks.length > 3) {
        this.tasks.splice(index, 1);
      }
    }
  }
};
</script>

<template>
  <div class="form">
    <h2>Добавить задачу</h2>
    <input class="task-title" type="text" v-model="title" placeholder="Введите заголовок" />

    <div class="task-row" v-for="(task, index) in tasks" :key="index">
      <input type="text" v-model="tasks[index]" placeholder="Введите задачу" />
      <button
          v-if="tasks.length > 3 && index >= 3"
          @click="removeTask(index)"
          class="remove-btn"
      >−</button>
    </div>

    <div class="button-group">
      <button v-if="tasks.length < 5" @click="addTask">Добавить пункт</button>
      <button @click="addCard" :disabled="!isValid">Добавить</button>
    </div>
  </div>
</template>

<style scoped>
  .form {
    background: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
    max-width: 400px;
    margin: 20px auto;
    text-align: center;
  }

  .task-title {
    margin-bottom: 20px;
  }

  .task-row {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-bottom: 10px;
  }

  input[type="text"] {
    flex-grow: 1;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 16px;
  }

  .remove-btn {
    background: red;
    color: white;
    border: none;
    padding: 8px 12px;
    cursor: pointer;
    border-radius: 5px;
    font-size: 18px;
    transition: background 0.2s ease-in-out;
  }

  .remove-btn:hover {
    background: darkred;
  }

  .button-group {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin-top: 15px;
  }

  button {
    background: #007bff;
    color: white;
    border: none;
    padding: 10px 15px;
    cursor: pointer;
    border-radius: 5px;
    font-size: 16px;
    transition: background 0.2s ease-in-out;
  }

  button:hover {
    background: #0056b3;
  }

  button:disabled {
    background: #ccc;
    cursor: not-allowed;
  }
</style>
