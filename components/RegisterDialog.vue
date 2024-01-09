<template>
  <div id="register-dialog" class="dialog">
    <div class="dialog-header">
      <h2>Register for ToDo</h2>
    </div>
    <div class="dialog-body">
      <div class="field">
        <label for="user-name">User Name *</label>
        <input
          type="text"
          name="user-name"
          id="register-user-name"
          v-model="credentials.UserName"
        />
      </div>
      <div class="field">
        <label for="pass-word">Pass Word *</label>
        <input
          type="password"
          name="pass-word"
          id="register-pass-word"
          v-model="credentials.PassWord"
        />
      </div>
      <div class="field">
        <label for="email">Email</label>
        <input
          type="email"
          name="email"
          id="register-email"
          v-model="credentials.Email"
        />
      </div>
      <div>* Required</div>
    </div>
    <div class="dialog-buttons">
      <button @click="cancel">Cancel</button>
      <button @click="register">Register</button>
    </div>
  </div>
</template>

<script lang="ts" setup>
const credentials = {
  UserName: "",
  PassWord: "",
  Email: "",
};
const emit = defineEmits(["cancel", "register"]);

const cancel = () => {
  emit("cancel");
};

const register = () => {
  if (credentials.UserName && credentials.PassWord)
    emit("register", credentials);
};
</script>
