<template>
    <div class="submit-form">
        <div v-if="!submitted">
        <div class="form-group">
            <label for="title">Task</label>
            <input
            type="text"
            class="form-control"
            id="title"
            required
            v-model="todo.title"
            name="title"
            />
        </div>

        <div class="form-group">
            <label for="description">Description</label>
            <input
            class="form-control"
            id="todo"
            required
            v-model="todo.description"
            name="description"
            />
        </div>

        <button @click="saveTodo" class="btn btn-success">Submit</button>
        </div>

        <div v-else>
        <h4>Submit Success</h4>
        <button class="btn btn-success" @click="newtodo">Add New</button>
        </div>
    </div>
</template>

<script>
import TodoDataService from "../services/TodoDataService";

export default {
    name: "add-todo",
    data() {
        return {
        todo: {
            id: null,
            title: "",
            description: "",
            stauts: false
        },
        submitted: false
        };
    },
    methods: {
        savetodo() {
        var data = {
            title: this.todo.title,
            description: this.todo.description
        };

        TodoDataService.create(data)
            .then(response => {
            this.todo.id = response.data.id;
            console.log(response.data);
            this.submitted = true;
            })
            .catch(e => {
            console.log(e);
            });
        },
        
        newTodo() {
        this.submitted = false;
        this.todo = {};
        }
    }
};
</script>

<style>
.submit-form {
    max-width: 300px;
    margin: auto;
}
</style>