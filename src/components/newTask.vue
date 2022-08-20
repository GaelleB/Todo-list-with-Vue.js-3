<template>
  <div class="container p-5 rounded-3 ">
    <h1 class="mb-5 text-center">ToDo list with Vue.js</h1>

    <!-- <form> -->
    <div class="d-flex mb-5">
      <input v-model="newTodo" type="text" placeholder="Enter ToDo" class="form-control form-input me-3"/>
      <button type="submit" class="submit-btn px-3 py-2" @click="addTodo()">
        +
      </button>
    </div>

    <div
      class="row todo-list border shadow px-3 pt-3 pb-2 align-items-center mb-4" v-for="(todo, index) in todos" :key="index">
      <div class="col-7 text-start">
        <h5 :class="{ 'todo-finished': todo.status === 'finished' }">
          {{ todo.name }}
        </h5>
      </div>
      <div class="col-3">
          <div
            class="status-indicator mb-1 me-2"
            :class="{
              'status-indicator-todo': todo.status === 'toDo',
              'status-indicator-ongoing': todo.status === 'onGoing',
              'status-indicator-finished': todo.status === 'finished',
            }"
          ></div>
          <div class="status-text" @click="changeStatus(index)"
            :class="{
              'status-text-todo': todo.status === 'toDo',
              'status-text-ongoing': todo.status === 'onGoing',
              'status-text-finished': todo.status === 'finished',
            }"
          >
            <h5>{{ todo.status }}</h5>
          </div>
      </div>
      <div class="col-2 text-end action-btn">
        <div class="d-flex justify-content-end">
          <div @click="editTodo(index)">
            <i id="edit" class="uil uil-edit"></i>
          </div>
          <div @click="deleteTodo(index)">
            <i id="trash" class="uil uil-trash"></i>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ToDo",
  data() {
    return {
      newTodo: "",
      indexEditTodo: null,
      tempNameTodo: "",
      tempStatusTodo: "",
      todoStatus: ["toDo", "onGoing", "finished"],
      todos: [
      ],
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo.length === 0) return;
      if (this.indexEditTodo === null) {
        this.todos.push({
          name: this.newTodo,
          status: "toDo",
        });
      } else {
        this.todos[this.indexEditTodo].name = this.newTodo;
        this.indexEditTodo = null;
      }
      this.newTodo = "";
    },
    editTodo(index) {
      this.newTodo = this.todos[index].name;
      this.indexEditTodo = index;
    },
    deleteTodo(index) {
      this.todos.splice(index, 1);
    },
    changeStatus(index) {
      let statusIndex = this.todoStatus.indexOf(this.todos[index].status);
      if (++statusIndex > 2) statusIndex = 0;
      this.todos[index].status = this.todoStatus[statusIndex];
    },
  },
};
</script>

<style scoped>
.form-input {
  margin-top: 5px;
  padding: 20px;
  padding-left: 10px;
  border: 3px solid coral;
  border-radius: 40px;
}
.form-control:focus {
  box-shadow: none;
  /* border: none; */
}
.submit-btn {
  background-color: coral;
  margin-left: 10px;
  border-radius: 50%;
  border: none;
  font-size: 20px;
  font-weight: 800;
  color: black;
}
.todo-list {
  border-radius: 50px;
}
.todo-finished {
  text-decoration: line-through;
}
.status-indicator-todo {
  background: coral;
  text-align: center;
}
.status-indicator-ongoing {
  background: orange;
  text-align: center;
}
.status-indicator-finished {
  background: cyan;
  text-align: center;
}
.status-text {
  font-weight: bold;
  cursor: pointer;
}
.status-text-todo {
  color: coral;
}
.status-text-ongoing {
  color: orange;
}
.status-text-finished {
  color: cyan;
}
.action-btn i {
  font-size: 30px;
}
</style>