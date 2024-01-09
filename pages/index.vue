<template>
  <div>
    <div v-if="session.SignedIn">
      <UserControls />
      <TodoList />
    </div>
    <div v-else>
      <WelcomeMessage />
    </div>
  </div>
</template>

<script lang="ts" setup>
const session = useSessionStore();
</script>
