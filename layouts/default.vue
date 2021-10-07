<template>
	<v-app>
		<v-app-bar app>
			<nuxt-link to="/" style="color: inherit; text-decoration: none">
				<v-toolbar-title v-text="title" />
			</nuxt-link>
			<v-spacer />
			<v-btn to="/recipes" text>Recipes</v-btn>
			<v-btn v-if="signedIn" @click="signout" text>Sign Out</v-btn>
			<v-btn
				v-if="$vuetify.breakpoint.mdAndUp"
				@click="signin"
				dark
				color="indigo"
				rounded
			>
				<v-icon dark>mdi-plus</v-icon>
				Add
			</v-btn>
		</v-app-bar>
		<v-main>
			<v-container>
				<Nuxt />
			</v-container>
			<v-btn
				v-if="$vuetify.breakpoint.smAndDown"
				@click="add"
				fab
				dark
				color="indigo"
				fixed
				right="20"
				bottom="20"
			>
				<v-icon dark>mdi-plus</v-icon>
			</v-btn>
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
		add() {
			if (this.signedIn) {
				window.location.assign(window.location.origin + "/admin/");
			} else {
				netlifyIdentity.open();
			}
		},
		signin() {
			netlifyIdentity.open();
		},
		signout() {
			netlifyIdentity.logout();
		},
	},
	mounted() {
		// let user = JSON.parse(localStorage.getItem("netlify-cms-user"));
		let signedIn = localStorage.getItem("gotrue.user");
		this.signedIn = signedIn ? true : false;
		window.addEventListener("load", () => {
			netlifyIdentity.on("login", () => {
				this.signedIn = true;
				window.location.assign(window.location.origin + "/admin/");
			});
			netlifyIdentity.on("logout", () => {
				this.signedIn = false;
				this.$router.push("/");
			});
		});
	},
};
</script>
