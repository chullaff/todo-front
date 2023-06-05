<template>
  <v-app>
    <v-main>
      <v-container>
        <v-row>
          <v-col col="7">
            <ToDoList
              v-bind:todoList="todoList"
              @setUpdateTodo="setUpdateTodo"
              @deleteTodo="deleteTodoBI"
            />
          </v-col>
          <v-col col="3">
            <v-text-field
              width="400px"
              label="Title"
              v-model="inputTitle"
              outlined
              class="shrink"
            >
            </v-text-field>
            <v-text-field
              width="400px"
              label="Description"
              v-model="inputDescription"
              outlined
              class="shrink"
            >
            </v-text-field>
            <v-btn
              v-if="!isUpdate"
              @click="createTodo()"
              style="margin-right: 10px"
              small
              color="green"
              :disabled="inputTitle.length && inputDescription.length <= 0"
              >Создать
            </v-btn>
            <v-btn
              v-if="isUpdate"
              @click="updateTodo()"
              small
              color="yellow"
              :disabled="inputTitle.length && inputDescription.length <= 0"
              >Обновить
            </v-btn>
            <v-btn @click="deleteTodos()" style="align: center" color="red"
              >Удалить всё
            </v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import ToDoList from "./components/ToDoList.vue";
const axios = require("axios").default;
const url = "http://localhost:3100/todos";

export default {
  name: "App",

  components: {
    ToDoList,
  },

  data: () => ({
    isUpdate: false,
    inputTitle: "",
    inputDescription: "",
    updateId: "",
    todoList: [],
  }),
  methods: {
    reset() {
      this.inputTitle = "";
      this.inputDescription = "";
      this.isUpdate = false;
      this.updateId = "";
    },

    async getTodos() {
      const newTodo = await axios.get(url);
      this.todoList = newTodo.data.todoList;
    },

    async deleteTodos() {
      await axios.delete(url);
      this.reset();
      this.getTodos();
    },

    async deleteTodoBI(id) {
      await axios.delete(url + "/" + id);
      this.reset();
      this.getTodos();
    },

    async createTodo() {
      await axios.post(url, {
        title: this.inputTitle,
        description: this.inputDescription,
      });
      this.reset();
      this.getTodos();
    },

    async setUpdateTodo(id) {
      this.isUpdate = true;
      const todo = await axios.get(url + "/" + id);
      this.inputTitle = todo.data.todo.title;
      this.inputDescription = todo.data.todo.description;
      this.updateId = id;
    },

    async updateTodo() {
      await axios.patch(url + "/" + this.updateId, {
        title: this.inputTitle,
        description: this.inputDescription,
      });
      this.reset();
      this.getTodos();
    },
  },
};
</script>
