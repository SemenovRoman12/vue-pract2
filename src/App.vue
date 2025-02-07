<script>
  import Columns from "@/components/Columns.vue";
  import CardForm from "@/components/CardForm.vue";

  export default {
    name: "App",
    components: { Columns, CardForm },
    methods: {
      onAddCard(newCard) {
        this.$refs.columns.addCard(newCard);
      }
    }
  };
</script>

<template>
  <div class="container">
    <h1>Менеджер задач</h1>
    <CardForm @add-card="onAddCard"/>
    <Columns ref="columns"/>
  </div>
</template>

<style scoped>
  .container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
    text-align: center;
  }

  h1 {
    color: #333;
    font-size: 24px;
    margin-bottom: 20px;
    font-weight: bold;
  }
</style>
