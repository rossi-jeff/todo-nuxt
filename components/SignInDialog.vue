<template>
  <div id="sign-in-dialog" class="dialog">
    <div class="dialog-header">
      <h2>Sign In to ToDo</h2>
    </div>
    <div class="dialog-body">
      <div class="field">
        <label for="user-name">User Name</label>
        <input
          type="text"
          name="user-name"
          id="sign-in-user-name"
          v-model="credentials.UserName"
        />
      </div>
      <div class="field">
        <label for="pass-word">Pass Word</label>
        <input
          type="password"
          name="pass-word"
          id="sign-in-pass-word"
          v-model="credentials.PassWord"
        />
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
  PassWord: "",
};
const emit = defineEmits(["cancel", "signIn"]);

const cancel = () => {
  emit("cancel");
};

const signIn = () => {
  if (credentials.UserName && credentials.PassWord) emit("signIn", credentials);
};
</script>
