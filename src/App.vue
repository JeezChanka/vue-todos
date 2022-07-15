<script>
import TodoList from "./components/TodoList.vue";

export default {
  components: {
    TodoList,
  },
  data() {
    return {
      todos: [],
      activeTab: "All",
      newTodo: "",
      activeFilter: (todo) => true,
    };
  },
  mounted() {
    fetch("http://localhost:4000/api/todos")
      .then((response) => response.json())
      .then((recived) => {
        this.todos = [];
        this.todos.push(...recived);
      });
  },
  methods: {
    deleteTodo(todo) {
      fetch(`http://localhost:4000/api/todos/${todo.id}`, {
        method: "DELETE",
      })
        .then((response) => response.json())
        .then((recived) => {
          this.todos = [];
          this.todos.push(...recived);
        });
    },
    update(todo) {
      fetch(`http://localhost:4000/api/todos/${todo.id}`, {
        method: "PATCH",
      })
        .then((response) => response.json())
        .then((recived) => {
          this.todos = [];
          this.todos.push(...recived);
        });
    },
    add() {
      fetch("http://localhost:4000/api/todos", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          text: this.newTodo,
        }),
      })
        .then((response) => response.json())
        .then((recived) => {
          this.todos = [];
          this.todos.push(...recived);
          this.newTodo = "";
        });
    },
    listAll() {
      this.activeTab = "All";
      this.activeFilter = (todo) => true;
    },
    listDone() {
      this.activeTab = "Done";
      this.activeFilter = (todo) => todo.done == true;
    },
    listDoing() {
      this.activeTab = "Doing";
      this.activeFilter = (todo) => todo.done == false;
    },
  },
};
</script>

<template>
  <div class="container">
    <div
      class="container rounded-3 border-2 border-dark my-5 bg-white"
      style="height: auto"
    >
      <div>
        <h1 style="font-size: 36px">Vue TODO list</h1>
        <br />
        <div class="row">
          <div class="col-8">
            <input
              class="py-3 form-control shadow"
              placeholder="Dodaj zadanie"
              type="text"
              v-model="newTodo"
            />
          </div>
          <div class="col-2">
            <button class="mt-2 btn btn-success btn-lg" @click="add(newTodo)">
              Dodaj
            </button>
          </div>
        </div>
      </div>
      <br />
      <hr />
      <div class="row rounded bg-white">
        <div class="col-12">
          <ul class="nav nav-pills nav-fill">
            <li class="nav-item">
              <a
                class="nav-link nav-link-color"
                v-bind:class="{ active: activeTab == 'All' }"
                @click="listAll()"
                href="#"
                >Wszystkie</a
              >
            </li>
            <li class="nav-item">
              <a
                class="nav-link nav-link-color"
                v-bind:class="{ active: activeTab == 'Doing' }"
                @click="listDoing()"
                href="#"
                >Wykonywane</a
              >
            </li>
            <li class="nav-item">
              <a
                class="nav-link nav-link-color"
                v-bind:class="{ active: activeTab == 'Done' }"
                @click="listDone()"
                href="#"
                >Zrobione</a
              >
            </li>
          </ul>
        </div>
      </div>
      <hr />
      <br />
      <TodoList
        v-for="todo in todos"
        :key="todo.id"
        :todo="todo"
        :activeFilter="activeFilter"
        @update="update"
        @delete="deleteTodo"
      >
      </TodoList>
    </div>
  </div>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}
</style>
