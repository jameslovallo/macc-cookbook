<template>
	<v-card height="80vh" style="display: flex; flex-direction: column">
		<v-card-title>
			<h2>Add a Recipe</h2>
		</v-card-title>
		<iframe :src="adminURL" width="100%" frameborder="0" style="flex-grow: 1" />
	</v-card>
</template>

<script>
export default {
	data: () => ({ adminURL: "" }),
	mounted() {
		this.adminURL = window.location.origin + "/admin";
	},
};
</script>
