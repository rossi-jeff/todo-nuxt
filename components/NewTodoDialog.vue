<template>
  <div id="new-todo-dialog" class="dialog w-96">
    <div class="dialog-header">
      <h2>Create a New ToDo</h2>
    </div>
    <div class="dialog-body">
      <div class="field">
        <label for="task" class="block">Task</label>
        <textarea name="task" id="new-todo-task" v-model="todo.Task"></textarea>
      </div>
      <div class="field">
        <div class="flex">
          <div>
            <input
              type="checkbox"
              name="completed"
              id="new-task-completed"
              class="inline-block mr-2"
              v-model="todo.Completed"
            />
          </div>
          <div>
            <label for="completed">Completed</label>
          </div>
        </div>
      </div>
    </div>
    <div class="dialog-buttons">
      <button @click="cancel">Cancel</button>
      <button @click="addTodo">Add Todo</button>
    </div>
  </div>
</template>

<script lang="ts" setup>
const todo = reactive({ Task: "", Completed: false });
const emit = defineEmits(["cancel", "addTodo"]);

const cancel = () => {
  emit("cancel");
};

const addTodo = () => {
  const { Task, Completed } = todo;
  emit("addTodo", { Task, Completed });
  todo.Task = "";
  todo.Completed = false;
};
</script>

<style scoped>
label {
  @apply font-bold text-base;
}
</style>
