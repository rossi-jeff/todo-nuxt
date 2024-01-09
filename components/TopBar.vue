<template>
  <div>
    <div id="top-bar">
      <h1>Todo App</h1>
      <div v-if="session.SignedIn">
        <span>{{ session.UserName }}</span>
        <button @click="signOut">Sign Out</button>
      </div>
      <div v-else>
        <button @click="showRegister">Register</button>
        <button @click="showSignIn">Sign In</button>
        <button @click="showRandom">Random</button>
      </div>
    </div>
    <div id="modal-overlay">
      <RegisterDialog @cancel="hideRegister" @register="register" />
      <SignInDialog @cancel="hideSignIn" @sign-in="signIn" />
      <RandomDialog
        @cancel="hideRandom"
        @sign-in="signInRandom"
        :userNames="userNames"
      />
    </div>
  </div>
</template>

<script lang="ts" setup>
import { blankSession, useSessionStore } from "../utils/session.store";
const session = useSessionStore();
import { buildHeaders } from "../utils/build-headers";
const { data: userNames } = await useFetch(`${baseUrl}/user`, {
  transform(data: any) {
    const filtered = data.filter((item: any) => item.Random);
    return filtered.map((item: any) => item.UserName);
  },
});

const showOverlay = () => {
  const overlay = document.getElementById("modal-overlay");
  if (overlay) overlay.classList.add("open");
};

const hideOverlay = () => {
  const overlay = document.getElementById("modal-overlay");
  if (overlay) overlay.classList.remove("open");
};

const showRegister = () => {
  const dialog = document.getElementById("register-dialog");
  if (dialog) dialog.classList.add("open");
  showOverlay();
};

const hideRegister = () => {
  const dialog = document.getElementById("register-dialog");
  if (dialog) dialog.classList.remove("open");
  hideOverlay();
};

const showSignIn = () => {
  const dialog = document.getElementById("sign-in-dialog");
  if (dialog) dialog.classList.add("open");
  showOverlay();
};

const hideSignIn = () => {
  const dialog = document.getElementById("sign-in-dialog");
  if (dialog) dialog.classList.remove("open");
  hideOverlay();
};

const showRandom = () => {
  const dialog = document.getElementById("random-dialog");
  if (dialog) dialog.classList.add("open");
  showOverlay();
};

const hideRandom = () => {
  const dialog = document.getElementById("random-dialog");
  if (dialog) dialog.classList.remove("open");
  hideOverlay();
};

const signInRandom = async (event: { UserName: string; PassWord: string }) => {
  const { UserName, PassWord } = event;
  const result = await fetch(`${baseUrl}/auth/login`, {
    method: "POST",
    body: JSON.stringify({ UserName, PassWord }),
    headers: buildHeaders(blankSession),
  });
  if (result.ok) {
    const user: { UserName: string; Token: string } = await result.json();
    const { UserName, Token } = user;
    session.$patch({ UserName, Token, SignedIn: true });
    hideRandom();
  } else {
    console.log(result);
  }
};

const signIn = async (event: { UserName: string; PassWord: string }) => {
  const { UserName, PassWord } = event;
  const result = await fetch(`${baseUrl}/auth/login`, {
    method: "POST",
    body: JSON.stringify({ UserName, PassWord }),
    headers: buildHeaders(blankSession),
  });
  if (result.ok) {
    const user: { UserName: string; Token: string } = await result.json();
    const { UserName, Token } = user;
    session.$patch({ UserName, Token, SignedIn: true });
    hideSignIn();
  } else {
    console.log(result);
  }
};

const register = async (event: {
  UserName: string;
  PassWord: string;
  Email: string;
}) => {
  const { UserName, PassWord, Email } = event;
  const result = await fetch(`${baseUrl}/auth/register`, {
    method: "POST",
    body: JSON.stringify({ UserName, PassWord, Email }),
    headers: buildHeaders(blankSession),
  });
  if (result.ok) {
    const user = await result.json();
    console.log(user);
    hideRegister();
  } else {
    console.log(result);
  }
};

const signOut = () => {
  session.$patch(blankSession);
};
</script>

<style>
#top-bar {
  @apply h-12 px-4 py-2 flex justify-between;
}
#top-bar button {
  @apply ml-2;
}
#modal-overlay {
  @apply fixed inset-0 bg-gray-500 bg-opacity-70 overflow-y-auto w-full h-full z-30 hidden;
}
#modal-overlay.open {
  @apply block;
}
.dialog {
  @apply relative mx-auto top-20 w-72 bg-white border border-black rounded p-2 hidden;
}
.dialog.open {
  @apply block;
}
.dialog-header {
  @apply text-white p-2 rounded bg-blue-800;
}
.dialog-header h2 {
  @apply text-white;
}
.field {
  @apply pt-2;
}
label {
  @apply font-bold block text-base;
}
input,
select {
  @apply border border-black rounded py-1 px-2 text-sm w-full;
}
.dialog-buttons {
  @apply flex justify-between mt-2;
}
</style>
