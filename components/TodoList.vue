<template>
  <div>
    <button @click="showNew">New Todo</button>
    <div id="todo-modal-overlay">
      <NewTodoDialog @cancel="hideNew" @add-todo="addTodo" />
    </div>
    <div v-if="todos && todos.length > 0">Double click todo text to edit</div>
    <TodoCard
      v-for="todo of todos"
      :key="todo.Id"
      :todo="todo"
      @set-completed="setCompleted"
      @set-task="setTask"
      @delete="deleteTodo"
    />
  </div>
</template>

<script lang="ts" setup>
const session = useSessionStore();
const { data: todos, refresh } = useFetch<Todo[]>(`${baseUrl}/todo`, {
  headers: buildHeaders(session),
});

const getTodoById = (Id: string) => {
  if (todos && todos.value) {
    for (let i = 0; i < todos.value.length; i++) {
      const todo = todos.value[i];
      if (todo.Id == Id) return todo;
    }
  }
  return null;
};

const setCompleted = async (event: { Id: string; Completed: boolean }) => {
  const { Id, Completed } = event;
  const todo = getTodoById(Id);
  if (!todo) return;
  const { Task } = todo;
  const result = await fetch(`${baseUrl}/todo/${Id}`, {
    method: "PATCH",
    body: JSON.stringify({ Completed, Task }),
    headers: buildHeaders(session),
  });
  if (result.ok) {
    refresh();
  }
};

const setTask = async (event: { Id: string; Task: string }) => {
  const { Id, Task } = event;
  const todo = getTodoById(Id);
  if (!todo) return;
  const { Completed } = todo;
  const result = await fetch(`${baseUrl}/todo/${Id}`, {
    method: "PATCH",
    body: JSON.stringify({ Completed, Task }),
    headers: buildHeaders(session),
  });
  if (result.ok) {
    refresh();
  }
};

const deleteTodo = async (event: { Id: string }) => {
  const { Id } = event;
  const result = await fetch(`${baseUrl}/todo/${Id}`, {
    method: "DELETE",
    headers: buildHeaders(session),
  });
  if (result.ok) {
    refresh();
  }
};

const showOverlay = () => {
  const overlay = document.getElementById("todo-modal-overlay");
  if (overlay) overlay.classList.add("open");
};

const hideOverlay = () => {
  const overlay = document.getElementById("todo-modal-overlay");
  if (overlay) overlay.classList.remove("open");
};

const showNew = () => {
  const dialog = document.getElementById("new-todo-dialog");
  if (dialog) dialog.classList.add("open");
  showOverlay();
};

const hideNew = () => {
  const dialog = document.getElementById("new-todo-dialog");
  if (dialog) dialog.classList.remove("open");
  hideOverlay();
};

const addTodo = async (event: { Task: string; Completed: boolean }) => {
  const { Task, Completed } = event;
  const result = await fetch(`${baseUrl}/todo`, {
    method: "POST",
    body: JSON.stringify({ Task, Completed }),
    headers: buildHeaders(session),
  });
  if (result.ok) {
    refresh();
    hideNew();
  }
};
</script>

<style>
#todo-modal-overlay {
  @apply fixed inset-0 bg-gray-500 bg-opacity-70 overflow-y-auto w-full h-full z-30 hidden;
}
#todo-modal-overlay.open {
  @apply block;
}
</style>
