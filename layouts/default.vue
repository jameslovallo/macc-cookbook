<template>
	<v-app>
		<v-app-bar app>
			<nuxt-link to="/" style="color: inherit; text-decoration: none">
				<v-toolbar-title v-text="title" />
			</nuxt-link>
			<v-spacer />
			<v-btn to="/recipes" text> Recipes </v-btn>
			<v-btn v-if="!signedIn" @click="signin" text>Sign In</v-btn>
			<v-btn v-else @click="signout" text>Sign Out</v-btn>
		</v-app-bar>
		<v-main>
			<v-container>
				<Nuxt />
			</v-container>
		</v-main>
	</v-app>
</template>

<script>
export default {
	data() {
		return {
			signedIn: false,
			title: "MACC Cookbook",
		};
	},
	methods: {
		signin() {
			netlifyIdentity.open();
		},
		signout() {
			netlifyIdentity.logout();
		},
	},
	mounted() {
		let user = JSON.parse(localStorage.getItem("netlify-cms-user"));
		let signedIn = localStorage.getItem("gotrue.user");
		this.signedIn = signedIn ? true : false;
		window.addEventListener("load", () => {
			netlifyIdentity.on("login", () => {
				this.signedIn = true;
				window.location.href = window.location.hostname + "/admin/";
			});
			netlifyIdentity.on("logout", () => {
				this.signedIn = false;
				this.$router.push("/");
			});
		});
	},
};
</script>
