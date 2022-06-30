<template>
  <div class="list row">
    <div class="col-md-8">
      <div class="input-group mb-3">
        <input type="text" class="form-control" placeholder="Search..."
          v-model="title"/>
        <div class="input-group-append">
          <button class="btn btn-outline-secondary" type="button"
            @click="searchTitle"
          >
            Search
          </button>
        </div>
      </div>
    </div>
    <div class="col-md-6">
      <h4>Todo List</h4>
      <ul class="list-group">
        <li class="list-group-item"
          :class="{ active: index == currentIndex }"
          v-for="(todo, index) in todos"
          :key="index"
          @click="setActiveTodo(todo, index)"
        >
          {{ todo.title }}
        </li>
      </ul>

      <button class="m-3 btn btn-sm btn-danger" @click="removeAllTodos">
        Delete all todos
      </button>
    </div>
    <div class="col-md-6">
      <div v-if="currentTodo">
        <h4>Details</h4>
        <div>
          <label><strong>Mission:</strong></label> {{ currentTodo.title }}
        </div>
        <div>
          <label><strong>Details:</strong></label> {{ currentTodo.description }}
        </div>
        <div>
          <label><strong>Status:</strong></label> {{ currentTodo.stauts ? "done" : "undone" }}
        </div>

        <router-link :to="'/todos/' + currentTodo.id" class="badge badge-warning">Edit</router-link>
      </div>
      <div v-else>
        <br />
        <p>Click right for details</p>
      </div>
    </div>
  </div>
</template>

<script>
import TodoDataService from "../services/TodoDataService";

export default {
  name: "todos-list",
  data() {
    return {
      todos: [],
      currentTodo: null,
      currentIndex: -1,
      title: ""
    };
  },
  methods: {
    retrieveTodos() {
      TodoDataService.getAll()
        .then(response => {
          this.todos = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },

    refreshList() {
      this.retrieveTodos();
      this.currentTodo = null;
      this.currentIndex = -1;
    },

    setActiveTodo(todo, index) {
      this.currentTodo = todo;
      this.currentIndex = todo ? index : -1;
    },

    removeAllTodos() {
      TodoDataService.deleteAll()
        .then(response => {
          console.log(response.data);
          this.refreshList();
        })
        .catch(e => {
          console.log(e);
        });
    },
    
    searchTitle() {
      TodoDataService.findByTitle(this.title)
        .then(response => {
          this.todos = response.data;
          this.setActiveTodo(null);
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    }
  },
  mounted() {
    this.retrieveTodos();
  }
};
</script>

<style>
.list {
  text-align: left;
  max-width: 750px;
  margin: auto;
}
</style>