<template>
  <section v-if="todos.length">
    <div v-show="filteredTodos.length" class="text-end">
      <div class="form-check complete-all">
        <input
          class="form-check-input"
          type="checkbox"
          v-model="allCompleted"
          id="completeAll"
        />
        <label class="form-check-label" for="completeAll">
          Mark all completed
        </label>
      </div>
    </div>
    <TodoItem
      class="todo-wrapper"
      v-for="todo in filteredTodos"
      :key="todo.id"
      v-bind="todo"
      v-model:completed="todo.completed"
      @remove="removeTask($event)"
    />
    <div class="no-todos" v-show="filteredTodos.length === 0">
      No task to show
    </div>
    <footer class="todos-footer">
      <span>{{ todoCount }}</span>
      <div
        class="btn-group btn-group-sm todo-visibility"
        role="group"
        aria-label="Todo Selection"
      >
        <button
          type="button"
          class="btn btn-outline-secondary"
          :class="{ selected: visibility === 'All' }"
          @click="changeVisibility('All')"
        >
          All
        </button>
        <button
          type="button"
          class="btn btn-outline-secondary"
          :class="{ selected: visibility === 'Active' }"
          @click="changeVisibility('Active')"
        >
          Active
        </button>
        <button
          type="button"
          class="btn btn-outline-secondary"
          :class="{ selected: visibility === 'Completed' }"
          @click="changeVisibility('Completed')"
        >
          Completed
        </button>
      </div>
    </footer>
  </section>
  <div class="no-todos" v-else>No task to show</div>
</template>

<script>
import TodoItem from "./TodoItem.vue";
import cachedTodos from "../assets/todos.json";

export default {
  name: "TodoList",
  components: {
    TodoItem,
  },
  data() {
    return {
      todos: cachedTodos,
      visibility: "All",
      allCompleted: false,
    };
  },
  methods: {
    addTask(newTask) {
      this.todos = [newTask, ...this.todos];
      if (this.visibility === "Completed") {
        this.visibility = "All";
      }
    },
    removeTask(taskId) {
      this.todos = this.todos.filter(({ id }) => id !== taskId);
    },
    changeVisibility(visibility) {
      this.visibility = visibility;
    },
  },
  computed: {
    todoCount() {
      if (this.filteredTodos.length) {
        return `${this.filteredTodos.length} item${
          this.filteredTodos.length > 1 ? "s" : ""
        }`;
      } else {
        return "";
      }
    },
    filteredTodos() {
      switch (this.visibility) {
        case "Active":
          return this.todos.filter((todo) => !todo.completed);
        case "Completed":
          return this.todos.filter((todo) => todo.completed);
        default:
          return this.todos;
      }
    },
  },
  watch: {
    allCompleted: {
      immediate: false,
      deep: false,
      handler(completed) {
        this.todos = this.todos.map((todo) => ({ ...todo, completed }));
      },
    },
  },
};
</script>