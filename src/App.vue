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
        <list
          :todos="todos.list"
          @deleteTodo="deleteTodoP"
          @doneTodo="doneTodoP"
        />
        <small>Total TODO: {{ totalTODO }}</small>
      </div>
    </div>
  </div>
</template>
<script>
import { ref, reactive, onMounted, computed } from "vue";
import List from "./components/List.vue";

export default {
  components: { List },
  setup() {
    const todo = ref("");
    const todos = reactive({
      list: [],
    });

    onMounted(() => {
      const items = localStorage.getItem("todos");
      todos.list = items ? JSON.parse(items) : [];
    });

    const totalTODO = computed(() => {
      return todos.list.length;
    });

    const add = () => {
      todos.list.unshift({
        activity: todo.value,
        isDone: false,
      });

      todo.value = "";
      saveToLocalStorage();
    };

    const deleteTodoP = (todoIndex) => {
      todos.list = todos.list.filter((item, index) => {
        if (index != todoIndex) {
          return item;
        }
      });
      saveToLocalStorage();
    };

    const doneTodoP = (todoIndex) => {
      todos.list = todos.list.filter((item, index) => {
        if (index == todoIndex) {
          item.isDone = true;
        }
        return item;
      });
      saveToLocalStorage();
    };

    const saveToLocalStorage = () => {
      localStorage.setItem("todos", JSON.stringify(todos.list));
    };

    return {
      todo,
      todos,
      totalTODO,
      add,
      deleteTodoP,
      doneTodoP,
    };
  },
};
</script>
