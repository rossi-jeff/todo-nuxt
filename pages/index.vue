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
useHead({
	title: 'ToDo App in Nuxt',
})
const session = useSessionStore()
</script>
