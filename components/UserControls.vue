<template>
  <div>
    <div v-if="currentUser && !currentUser.Random" class="w-full text-right">
      <button>Edit {{ session.UserName }}</button>
      <button class="ml-2">Change Password</button>
    </div>
  </div>
</template>

<script lang="ts" setup>
const session = useSessionStore();
const { data: currentUser } = useFetch<User>(`${baseUrl}/user/current`, {
  headers: buildHeaders(session),
});
</script>
