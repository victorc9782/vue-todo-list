<template>
  <div v-if="currentTodo" class="edit-form">
    <h4>Todo</h4>
    <form>
      <div class="form-group">
        <label for="title">Task：</label>
        <input type="text" class="form-control" id="title"
          v-model="currentTodo.title"
        />
      </div>
      <div class="form-group">
        <label for="description">Description：</label>
        <input type="text" class="form-control" id="description"
          v-model="currentTodo.description"
        />
      </div>

      <div class="form-group">
        <label><strong>Stauts:</strong></label>
        {{ currentTodo.stauts ? "done" : "undone" }}
      </div>
    </form>

    <button class="badge badge-primary mr-2"
      v-if="currentTodo.stauts"
      @click="updatestauts(false)"
    >
      Incompleted
    </button>
    <button v-else class="badge badge-primary mr-2"
      @click="updatestauts(true)"
    >
      Completed
    </button>

    <button class="badge badge-danger mr-2"
      @click="deleteTodo"
    >
      Delete
    </button>

    <button type="submit" class="badge badge-success"
      @click="updateTodo"
    >
      Update
    </button>
    <p>{{ message }}</p>
  </div>

  <div v-else>
    <br />
    <p>Please click on a Todo...</p>
  </div>
</template>

<script>
import TodoDataService from "../services/TodoDataService";

export default {
  name: "todo-item",
  data() {
    return {
      currentTodo: null,
      message: ''
    };
  },
  methods: {
    getTodo(id) {
      TodoDataService.get(id)
        .then(response => {
          this.currentTodo = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },

    updatestauts(status) {
      var data = {
        id: this.currentTodo.id,
        title: this.currentTodo.title,
        description: this.currentTodo.description,
        stauts: status
      };

      TodoDataService.update(this.currentTodo.id, data)
        .then(response => {
          console.log(response.data);
          this.currentTodo.stauts = status;
          this.message = 'Status updated。';
        })
        .catch(e => {
          console.log(e);
        });
    },

    updateTodo() {
      TodoDataService.update(this.currentTodo.id, this.currentTodo)
        .then(response => {
          console.log(response.data);
          this.message = 'Todo updated';
        })
        .catch(e => {
          console.log(e);
        });
    },

    deleteTodo() {
      TodoDataService.delete(this.currentTodo.id)
        .then(response => {
          console.log(response.data);
          this.$router.push({ name: "todos" });
        })
        .catch(e => {
          console.log(e);
        });
    }
  },
  mounted() {
    this.message = '';
    this.getTodo(this.$route.params.id);
  }
};
</script>

<style>
.edit-form {
  max-width: 300px;
  margin: auto;
}
</style>