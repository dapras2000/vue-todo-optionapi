<template>
  <div class="container" style="margin-top: 20px">
    <div class="card">
      <div class="card-body">
        <div class="card-title">SIMPLE TODO APP</div>
        <div class="row">
          <div class="col-10">
            <input
              v-model="todo"
              type="text"
              class="form-control"
              @keyup.enter="add"
            />
          </div>
          <div class="col-2">
            <div class="btn btn-success" @click="add">ADD</div>
          </div>
        </div>
        <list :todos="todos" @deleteTodo="deleteTodoP" @doneTodo="doneTodoP" />
        <small>Total TODO: {{ totalTODO }}</small>
      </div>
    </div>
  </div>
</template>
<script>
import List from "./components/List.vue";
export default {
  components: { List },
  data() {
    return {
      todo: "",
      todos: [],
    };
  },
  mounted() {
    this.todos = JSON.parse(localStorage.getItem("todos"));
  },
  computed: {
    totalTODO() {
      return this.todos.length;
    },
  },
  methods: {
    add() {
      //this.todos.push(this.todo);
      this.todos.unshift({
        activity: this.todo,
        isDone: false,
      });
      this.todo = "";
      this.saveToLocalStorage();
    },
    deleteTodoP(todoIndex) {
      this.todos = this.todos.filter((item, index) => {
        if (index != todoIndex) {
          return item;
        }
      });
      this.saveToLocalStorage();
    },
    doneTodoP(todoIndex) {
      this.todos = this.todos.filter((item, index) => {
        if (index == todoIndex) {
          item.isDone = true;
        }

        return item;
      });
      this.saveToLocalStorage();
    },
    saveToLocalStorage() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
  },
};
</script>
