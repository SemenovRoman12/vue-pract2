<script>
  import Card from "@/components/Card.vue";

  export default {
    name: "Columns",
    components: {Card},
    data() {
      return {
        columns: {
          todo: [],
          inProgress: [],
          done: [],
        },
      };
    },
    methods: {

    }
  }
</script>

<template>
  <div class="wrapper">
    <div class="column column_todo">
      <Card v-for="notes in columns.todo" :notes="notes"></Card>
    </div>
    <div class="column column_in-progress">
      <Card v-for="notes in columns.inProgress" :notes="notes"></Card>
    </div>
    <div class="column column_todo">
      <Card v-for="notes in columns.done" :notes="notes"></Card>
    </div>
  </div>
</template>

<style scoped>

</style>
