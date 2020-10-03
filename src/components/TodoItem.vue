<template>
  <div class="todo-item">
    <div class="form-check selectable">
      <input
        class="form-check-input"
        type="checkbox"
        @change="toggleCompleted"
        :checked="completed"
        :id="id"
      />
      <label
        class="form-check-label"
        :style="{ 'text-decoration': completed ? 'line-through' : '' }"
        :for="id"
      >
        {{ name }}
      </label>
    </div>
    <i
      @click="removeTask"
      title="Remove todo"
      class="fas fa-trash-alt selectable"
    ></i>
  </div>
</template>

<script>
export default {
  name: "TodoItem",
  props: {
    id: Number,
    name: String,
    completed: Boolean,
  },
  methods: {
    toggleCompleted() {
      this.$emit("update:completed", !this.completed);
    },
    removeTask() {
      this.$emit("remove", this.id);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.todo-item {
  display: flex;
  justify-content: space-between;
  padding: 10px;
  align-items: center;
  /* border-radius: 5px; */
  border: 1px solid #cacaca;
  background: #fff;
  margin-bottom: 5px;
  .selectable {
    cursor: pointer;
  }
}
</style>
