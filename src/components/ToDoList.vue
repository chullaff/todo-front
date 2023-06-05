<template>
  <v-card v-for="todo in todoList" :key="todo">
    <v-card-title>{{ todo.title }}</v-card-title>
    <v-card-text>{{ todo.description }}</v-card-text>
    <v-card-actions class="justify-end">
      <v-btn
        variant="text"
        color="teal-accent-4"
        @click="$emit('setUpdateTodo', todo.id)"
        icon="mdi-square-edit-outline"
      ></v-btn>
      <v-btn
        variant="text"
        color="teal-accent-4"
        @click="$emit('deleteTodo', todo.id)"
        icon="mdi-trash-can-outline"
      ></v-btn>
    </v-card-actions>
  </v-card>
</template>
<script>
export default {
  props: ["todoList"],
  methods: {},
};
</script>

<style scoped>
.v-card {
  margin-bottom: 10px;
}
</style>
