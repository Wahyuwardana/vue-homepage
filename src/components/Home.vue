<template>
  <div class="container shadow-lg px-5 py-5 rounded">
    <div class="header">
      <img class="justify-content-center" src="../assets/pngwing.com.png" width="200" height="200" alt="">
    </div>
    <div class="title mt-2 mb-5">
      <h2>Tugas Akhir Vue JS</h2>
    </div>
    <div class="pb-2 mb-3">
      <div class="card">
        <div class="card-body">
          <div class="d-flex flex-row align-items-center">
            <input v-model="newTodo" type="text" placeholder="Enter To Do" class="form-control form-input me-4" />
            <div>
              <button type="submit" class="submit-btn px-3 py-2" @click="addTodo()">+</button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- </form> -->
    <transition-group name="list" tag="div">
      <div class="row todo-list px-3 pt-3 pb-2 align-items-center mb-4" style="background-color: #80b3ff; color:white;"
        v-for="(todo, index) in todos" :key="index">
        <div class="col-7 text-start">
          <h5 :class="{ 'todo-finished': todo.status === 'Selesai' }">
            {{ todo.name }}
          </h5>
        </div>
        <div class="col-2">
          <div class="d-flex justify-content-start align-items-center text-center">
            <div class="status-indicator mb-1 me-2" :class="{
              'status-indicator-todo': todo.status === 'Melakukan',
              'status-indicator-ongoing': todo.status === 'Proses',
              'status-indicator-finished': todo.status === 'Selesai',
            }"></div>
            <div class="status-text" @click="changeStatus(index)" :class="{
              'status-text-todo': todo.status === 'Melakukan',
              'status-text-ongoing': todo.status === 'Proses',
              'status-text-finished': todo.status === 'Selesai',
            }">
              <h5>{{ todo.status }}</h5>
            </div>
          </div>
        </div>
        <div class="col-3 text-end action-btn">
          <div class="d-flex justify-content-end">
            <div class="" @click="editTodo(index)">
              <i class="uil uil-edit-alt ms-4"></i>
            </div>
            <div class="" @click="deleteTodo(index)">
              <i class="uil uil-trash-alt ms-4"></i>
            </div>
          </div>
        </div>
      </div>
    </transition-group>

  </div>
</template>

<script>
  export default {
    name: "Home",

    data() {
      return {
        newTodo: "",
        indexEditTodo: null,
        tempNameTodo: "",
        tempStatusTodo: "",
        todoStatus: ["Melakukan", "Proses", "Selesai"],
        todos: [{
          name: "Belajar VueJs 2",
          status: "Proses",
        }, ],
      };
    },

    methods: {
      addTodo() {
        if (this.newTodo.length === 0) return;

        if (this.indexEditTodo === null) {
          this.todos.push({
            name: this.newTodo,
            status: "Melakukan",
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
    border: none;
  }

  .form-control:focus {
    box-shadow: none;
    border-color: transparent;
    border: none;
  }

  .submit-btn {
    background-color: #80b3ff;
    border-radius: 10%;
    border: none;

    font-size: 20px;
    font-weight: 800;
    color: white;
  }

  .todo-list {
    border-radius: 10px;
  }

  .todo-finished {
    font-style: italic;
    text-decoration: line-through;
  }

  .status-indicator {
    width: 10px;
    height: 10px;
    border-radius: 50%;
  }

  .status-indicator-todo {
    background: red;
  }

  .status-indicator-ongoing {
    background: yellow;
  }

  .status-indicator-finished {
    background: green;
  }

  .status-text {
    font-weight: bold;
    cursor: pointer;
  }

  .status-text-todo {
    color: red;
  }

  .status-text-ongoing {
    color: yellow;
  }

  .status-text-finished {
    color: green;
  }

  .action-btn i {
    font-size: 25px;
    cursor: pointer;
  }

  .list-move,
  .list-enter-active,
  .list-leave-active {
    transition: all 0.5s ease;
  }

  .list-enter-from,
  .list-leave-to {
    opacity: 0;
    transform: translateX(40px);
  }
</style>