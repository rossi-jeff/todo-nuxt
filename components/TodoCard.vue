<template>
  <div class="todo-card">
    <div>
      <input
        type="checkbox"
        name="completed"
        v-model="props.todo.Completed"
        class="mr-2"
        @change="completedChanged"
      />
    </div>
    <div class="flex-grow">
      <div v-if="state.editing">
        <textarea name="task" v-model="props.todo.Task"></textarea>
        <div class="flex justify-between">
          <button @click="state.editing = false">Cancel</button>
          <button @click="updateTask">Update</button>
        </div>
      </div>
      <div v-else @dblclick="state.editing = true" class="cursor-pointer">
        {{ props.todo.Task }}
      </div>
    </div>
    <div>
      <button class="ml-2" @click="deleteTodo">Delete</button>
    </div>
  </div>
</template>

<script lang="ts" setup>
const props = defineProps<{ todo: Todo }>();
const emit = defineEmits(["setCompleted", "setTask", "delete"]);
const state = reactive({ editing: false });

const completedChanged = () => {
  const { Id, Completed } = props.todo;
  emit("setCompleted", { Id, Completed });
};

const updateTask = () => {
  const { Id, Task } = props.todo;
  emit("setTask", { Id, Task });
  state.editing = false;
};

const deleteTodo = () => {
  const { Id } = props.todo;
  emit("delete", { Id });
};
</script>

<style>
.todo-card {
  @apply flex p-2 mb-2 border border-black rounded;
}
textarea {
  @apply w-full border border-black rounded text-sm py-1 px-2;
}
</style>
