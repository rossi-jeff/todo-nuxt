<template>
  <div>
    <div v-if="currentUser && !currentUser.Random" class="w-full text-right">
      <button @click="showEdit">Edit {{ session.UserName }}</button>
      <button @click="showChange" class="ml-2">Change Password</button>
    </div>
    <div id="user-modal-overlay" v-if="currentUser && !currentUser.Random">
      <UserEditDialog
        @cancel="hideEdit"
        :user="currentUser"
        @update-user="updateUser"
      />
      <ChangePasswordDialog
        @cancel="hideChange"
        @change-password="changePassword"
      />
    </div>
  </div>
</template>

<script lang="ts" setup>
const session = useSessionStore();
const { data: currentUser, refresh } = useFetch<User>(
  `${baseUrl}/user/current`,
  {
    headers: buildHeaders(session),
  }
);

const showOverlay = () => {
  const overlay = document.getElementById("user-modal-overlay");
  if (overlay) overlay.classList.add("open");
};

const hideOverlay = () => {
  const overlay = document.getElementById("user-modal-overlay");
  if (overlay) overlay.classList.remove("open");
};

const showEdit = () => {
  const dialog = document.getElementById("edit-user-dialog");
  if (dialog) dialog.classList.add("open");
  showOverlay();
};

const hideEdit = () => {
  const dialog = document.getElementById("edit-user-dialog");
  if (dialog) dialog.classList.remove("open");
  hideOverlay();
};

const showChange = () => {
  const dialog = document.getElementById("change-password-dialog");
  if (dialog) dialog.classList.add("open");
  showOverlay();
};

const hideChange = () => {
  const dialog = document.getElementById("change-password-dialog");
  if (dialog) dialog.classList.remove("open");
  hideOverlay();
};

const updateUser = async (event: { UserName: string; Email: string }) => {
  if (!currentUser.value) return;
  const { UserName, Email } = event;
  const Id = currentUser.value.Id;
  const result = await fetch(`${baseUrl}/user/${Id}`, {
    method: "PATCH",
    body: JSON.stringify({ UserName, Email }),
    headers: buildHeaders(session),
  });
  if (result.ok) {
    refresh();
  }
  hideEdit();
};

const changePassword = async (event: {
  OldPassWord: string;
  NewPassWord: string;
  Confirmation: string;
}) => {
  console.log(event);
  const { OldPassWord, NewPassWord, Confirmation } = event;
  const result = await fetch(`${baseUrl}/user/change`, {
    method: "PATCH",
    body: JSON.stringify({ OldPassWord, NewPassWord, Confirmation }),
    headers: buildHeaders(session),
  });
  if (result.ok) {
    refresh();
  }
  hideChange();
};
</script>

<style>
#user-modal-overlay {
  @apply fixed inset-0 bg-gray-500 bg-opacity-70 overflow-y-auto w-full h-full z-30 hidden;
}
#user-modal-overlay.open {
  @apply block;
}
</style>
