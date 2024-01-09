<template>
	<div id="edit-user-dialog" class="dialog">
		<div class="dialog-header">
			<h2>Edit User</h2>
		</div>
		<div class="dialog-body">
			<div class="field">
				<label for="user-name">User Name</label>
				<input
					type="text"
					name="user-name"
					id="edit-user-user-name"
					v-model="user.UserName"
				/>
			</div>
			<div class="field">
				<label for="email">Email</label>
				<input
					type="email"
					name="email"
					id="edit-user-email"
					v-model="user.Email"
				/>
			</div>
		</div>
		<div class="dialog-buttons">
			<button @click="cancel">Cancel</button>
			<button @click="updateUser">Update User</button>
		</div>
	</div>
</template>

<script lang="ts" setup>
import type { User } from '../utils/user.type'

const emit = defineEmits(['cancel', 'updateUser'])
const props = defineProps<{ user: User }>()
const user = reactive({
	UserName: props.user.UserName,
	Email: props.user.Email,
})

const cancel = () => {
	emit('cancel')
}

const updateUser = () => {
	const { UserName, Email } = user
	emit('updateUser', { UserName, Email })
}
</script>
