<template>
  <div id="random-dialog" class="dialog">
    <div class="dialog-header">
      <h2>Random Sign In to ToDo</h2>
    </div>
    <div class="dialog-body">
      <div class="field">
        <label for="user-name">User Name</label>
        <select
          name="user-name"
          id="random-user-name"
          v-model="credentials.UserName"
        >
          <option value="">- select -</option>
          <option v-for="n of props.userNames" :value="n">{{ n }}</option>
        </select>
      </div>
    </div>
    <div class="dialog-buttons">
      <button @click="cancel">Cancel</button>
      <button @click="signIn">Sign In</button>
    </div>
  </div>
</template>

<script lang="ts" setup>
const credentials = {
  UserName: "",
  PassWord: randomUserPassword,
};
const emit = defineEmits(["cancel", "signIn"]);
const props = defineProps(["userNames"]);

const cancel = () => {
  emit("cancel");
};

const signIn = () => {
  if (credentials.UserName) emit("signIn", credentials);
};
</script>
