<script>
export default {
  name: "CardForm",
  data() {
    return {
      title: "",
      tasks: [
        { text: "", subnotes: [] },
        { text: "", subnotes: [] },
        { text: "", subnotes: [] }
      ],
      priority: 1
    };
  },
  computed: {
    isValid() {
      return (
          this.title.trim() !== "" &&
          this.tasks.every(task => task.text.trim() !== "")
      );
    }
  },
  methods: {
    addCard() {
      if (this.isValid) {
        this.$emit("add-card", {
          title: this.title,
          notes: this.tasks.map(task => ({
            text: task.text,
            done: false,
            subnotes: task.subnotes.map(sub => ({ text: sub.text, done: false }))
          })),
          priority: this.priority
        });
        this.resetForm();
      } else {
        alert("Введите заголовок и минимум 3 задачи!");
      }
    },
    addTask() {
      if (this.tasks.length < 5) {
        this.tasks.push({ text: "", subnotes: [] });
      }
    },
    removeTask(index) {
      if (this.tasks.length > 3) {
        this.tasks.splice(index, 1);
      }
    },
    addSubnote(task) {
      if (task.subnotes.length < 2) {
        task.subnotes.push({ text: "" });
      }
    },
    removeSubnote(task, index) {
      task.subnotes.splice(index, 1);
    },
    resetForm() {
      this.title = "";
      this.tasks = [
        { text: "", subnotes: [] },
        { text: "", subnotes: [] },
        { text: "", subnotes: [] }
      ];
      this.priority = 1;
    }
  }
};
</script>

<template>
  <div class="form">
    <h2 class="form-title">Добавить задачу</h2>
    <input
        class="form-input task-title"
        type="text"
        v-model="title"
        placeholder="Введите заголовок"
    />

    <div class="task-row" v-for="(task, index) in tasks" :key="index">
      <div class="task-container">
        <input
            class="form-input"
            type="text"
            v-model="task.text"
            placeholder="Введите задачу"
        />
        <button
            v-if="tasks.length > 3 && index >= 3"
            @click="removeTask(index)"
            class="remove-btn"
        >−</button>

        <div class="subnotes">
          <div v-for="(subnote, subIndex) in task.subnotes" :key="subIndex">
            <input
                class="form-input small"
                type="text"
                v-model="subnote.text"
                placeholder="Введите подпункт"
            />
            <button
                @click="removeSubnote(task, subIndex)"
                class="remove-btn small"
            >−</button>
          </div>
          <button
              v-if="task.subnotes.length < 2"
              @click="addSubnote(task)"
              class="add-subnote-btn"
          >
            Добавить подпункт
          </button>
        </div>
      </div>
    </div>

    <button class="add-task-btn" v-if="tasks.length < 5" @click="addTask">
      Добавить пункт
    </button>

    <h3 class="priority-title">Приоритет</h3>
    <select class="form-select" v-model="priority">
      <option :value="1">Высокий</option>
      <option :value="2">Средний</option>
      <option :value="3">Низкий</option>
    </select>

    <div class="button-group">
      <button class="submit-btn" @click="addCard" :disabled="!isValid">
        Добавить задачу
      </button>
    </div>
  </div>
</template>

<style scoped>
.form {
  background: #f9f9f9;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  max-width: 500px;
  margin: 20px auto;
  text-align: left;
  font-family: Arial, sans-serif;
}

.form-title {
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 20px;
  text-align: center;
}

.form-input {
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 14px;
}

.form-input.small {
  width: calc(100% - 50px);
}

.task-row {
  margin-bottom: 15px;
}

.task-title {
  margin-bottom: 30px;
}

.task-container {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.subnotes {
  padding-left: 20px;
  margin-top: 5px;
}

.remove-btn {
  background: red;
  color: white;
  border: none;
  padding: 5px 8px;
  cursor: pointer;
  border-radius: 5px;
  font-size: 14px;
  transition: background 0.2s ease-in-out;
}

.remove-btn.small {
  padding: 4px 6px;
  font-size: 12px;
}

.remove-btn:hover {
  background: darkred;
}


.add-subnote-btn {
  background: #28a745;
  color: white;
  border: none;
  padding: 6px 10px;
  cursor: pointer;
  border-radius: 5px;
  font-size: 14px;
  margin-top: 5px;
  transition: background 0.2s ease-in-out;
}

.add-subnote-btn:hover {
  background: #218838;
}

.add-task-btn {
  margin-top: 10px;
  background: #007bff;
  color: white;
  border: none;
  padding: 10px 15px;
  cursor: pointer;
  border-radius: 5px;
  font-size: 14px;
  transition: background 0.2s ease-in-out;
}

.add-task-btn:hover {
  background: #0056b3;
}

.priority-title {
  font-size: 16px;
  margin-top: 15px;
  margin-bottom: 5px;
  color: #333;
}

.form-select {
  width: 100%;
  padding: 10px;
  margin-bottom: 15px;
  border: 1px solid #ccc;
  border-radius: 5px;
}


.button-group {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.submit-btn {
  background: #28a745;
  color: white;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  border-radius: 5px;
  font-size: 16px;
  transition: background 0.2s ease-in-out;
}

.submit-btn:hover {
  background: #218838;
}

.submit-btn:disabled {
  background: #ccc;
  cursor: not-allowed;
}
</style>
