<template>
	<v-btn @click="login"> Log In </v-btn>
</template>

<script>
export default {
	head() {
		return {
			script: [
				{ src: "https://identity.netlify.com/v1/netlify-identity-widget.js" },
			],
		};
	},
	methods: {
		login() {
			netlifyIdentity.open();
		},
	},
	mounted() {
		netlifyIdentity.on("login", () => {
			this.$router.push("/admin");
		});
	},
};
</script>
