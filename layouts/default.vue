<template>
	<v-app>
		<v-app-bar app>
			<nuxt-link to="/" style="color: inherit; text-decoration: none">
				<v-toolbar-title v-text="title" />
			</nuxt-link>
			<v-spacer />
			<v-btn to="/recipes" text rounded>
				<v-icon left>mdi-magnify</v-icon>
				Search
			</v-btn>
			<v-btn
				v-if="$vuetify.breakpoint.mdAndUp"
				@click="signin"
				dark
				color="primary"
				rounded
			>
				<v-icon left>mdi-plus</v-icon>
				Add
			</v-btn>
		</v-app-bar>
		<v-main>
			<v-container class="mb-16">
				<Nuxt />
				<v-btn
					v-if="$vuetify.breakpoint.smAndDown"
					@click="signin"
					color="primary"
					fab
					dark
					fixed
					:right="true"
					:bottom="true"
				>
					<v-icon dark>mdi-plus</v-icon>
				</v-btn>
				<v-dialog
					v-model="showAdmin"
					fullscreen
					hide-overlay
					transition="dialog-bottom-transition"
				>
					<v-card height="80vh" style="display: flex; flex-direction: column">
						<v-toolbar
							flat
							style="border-bottom: 1px solid rgb(223, 223, 227); flex-grow: 0"
						>
							<v-toolbar-title>
								MACC Cookbook &nbsp;➜&nbsp; Add
							</v-toolbar-title>
							<v-spacer></v-spacer>
							<v-btn @click="showAdmin = false" color="red darken-2" text>
								<v-icon>mdi-close</v-icon>
								Close
							</v-btn>
						</v-toolbar>
						<iframe
							:src="adminURL"
							width="100%"
							frameborder="0"
							style="flex-grow: 1"
						/>
					</v-card>
				</v-dialog>
			</v-container>
		</v-main>
	</v-app>
</template>

<script>
export default {
	data: () => ({
		adminURL: "",
		showAdmin: false,
		signedIn: false,
		title: "MACC Cookbook",
	}),
	methods: {
		signin() {
			this.signedIn === true || window.location.hostname === "localhost"
				? (this.showAdmin = true)
				: netlifyIdentity.open();
		},
	},
	created() {},
	mounted() {
		const darkMode = window.matchMedia("(prefers-color-scheme: dark)").matches;
		if (darkMode) this.$vuetify.theme.dark = true;
		const signedIn = localStorage.getItem("gotrue.user");
		this.adminURL = window.location.origin + "/admin";
		this.signedIn = signedIn ? true : false;
		netlifyIdentity.on("login", () => {
			this.signedIn = true;
		});
		netlifyIdentity.on("logout", () => {
			this.signedIn = false;
		});
	},
};
</script>

<style>
	.v-dialog {
		margin: 0 !important;
	}
</style>
