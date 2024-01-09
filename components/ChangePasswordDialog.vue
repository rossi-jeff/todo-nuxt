<template>
	<div id="change-password-dialog" class="dialog">
		<div class="dialog-header">
			<h2>Change Password</h2>
		</div>
		<div class="dialog-body">
			<div class="field">
				<label for="old-password">Old Password</label>
				<input
					type="password"
					name="old-password"
					id="edit-old-password"
					v-model="pass.old"
				/>
			</div>
			<div class="field">
				<label for="new-password">New Password</label>
				<input
					type="password"
					name="new-password"
					id="edit-new-password"
					v-model="pass.new"
				/>
			</div>
			<div class="field">
				<label for="password-confirmation">Confirmation</label>
				<input
					type="password"
					name="password-confirmation"
					id="edit-password-confirmation"
					v-model="pass.confirm"
				/>
			</div>
		</div>
		<div class="dialog-buttons">
			<button @click="cancel">Cancel</button>
			<button @click="updateUser">Change Password</button>
		</div>
	</div>
</template>

<script lang="ts" setup>
const emit = defineEmits(['cancel', 'changePassword'])
const pass = reactive({ old: '', new: '', confirm: '' })

const cancel = () => {
	emit('cancel')
}

const updateUser = () => {
	if (pass.old && pass.new && pass.new == pass.confirm) {
		emit('changePassword', {
			OldPassWord: pass.old,
			NewPassWord: pass.new,
			Confirmation: pass.confirm,
		})
	}
}
</script>
