<template>
  <div class="container-fluid todo-content">
    <h1 class="todo-title">todos</h1>
    <div class="form-group">
      <input
        :placeholder="todoPlaceholder"
        v-model="task"
        type="text"
        class="form-control form-control-lg todo-input"
        @keyup.enter="addTask"
        @keyup.esc="clearTask"
      />
    </div>
    <div v-show="task" class="form-group">
      <label>Preview - </label>
      {{ task }}
    </div>
    <TodoList ref="todoList" />
  </div>
</template>

<script>
import TodoList from "./components/TodoList.vue";

export default {
  name: "App",
  components: {
    TodoList,
  },
  data() {
    return {
      todoPlaceholder: "What to do next?",
      task: undefined,
      newUid: 101,
    };
  },
  methods: {
    addTask() {
      if (this.task) {
        const newTask = {
          id: ++this.newUid,
          name: this.task,
          completed: false,
        };
        this.$refs.todoList.addTask(newTask);
        this.clearTask();
      }
    },
    clearTask() {
      this.task = null;
    },
  },
};
</script>

<style lang="scss">
@import "./styles/index.scss";
</style>
