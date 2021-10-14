<template>
	<v-app>
		<v-app-bar app>
			<nuxt-link to="/" style="color: inherit; text-decoration: none">
				<v-toolbar-title v-text="title" />
			</nuxt-link>
			<v-spacer />
			<v-btn to="/recipes" text>Find a Recipe</v-btn>
			<v-btn
				v-if="$vuetify.breakpoint.mdAndUp"
				@click="signin"
				dark
				color="primary"
				rounded
			>
				<v-icon dark left style="height: 24px; width: 24px">
					M15.54,3.5L20.5,8.47L19.07,9.88L14.12,4.93L15.54,3.5M3.5,19.78L10,13.31C9.9,13
					9.97,12.61 10.23,12.35C10.62,11.96 11.26,11.96 11.65,12.35C12.04,12.75
					12.04,13.38 11.65,13.77C11.39,14.03 11,14.1
					10.69,14L4.22,20.5L14.83,16.95L18.36,10.59L13.42,5.64L7.05,9.17L3.5,19.78Z
				</v-icon>
				Edit
			</v-btn>
		</v-app-bar>
		<v-main>
			<v-container class="mb-16">
				<Nuxt />
				<v-btn
					v-if="$vuetify.breakpoint.smAndDown"
					@click="signin"
					fab
					dark
					fixed
					:right="true"
					:bottom="true"
				>
					<v-icon dark>
						M15.54,3.5L20.5,8.47L19.07,9.88L14.12,4.93L15.54,3.5M3.5,19.78L10,13.31C9.9,13
						9.97,12.61 10.23,12.35C10.62,11.96 11.26,11.96
						11.65,12.35C12.04,12.75 12.04,13.38 11.65,13.77C11.39,14.03 11,14.1
						10.69,14L4.22,20.5L14.83,16.95L18.36,10.59L13.42,5.64L7.05,9.17L3.5,19.78Z</v-icon
					>
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
								MACC Cookbook &nbsp;➜&nbsp; Edit
							</v-toolbar-title>
							<v-spacer></v-spacer>
							<v-btn @click="showAdmin = false" color="red darken-2" text>
								<v-icon dark>mdi-close</v-icon>
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
	mounted() {
		this.adminURL = window.location.origin + "/admin";
		let signedIn = localStorage.getItem("gotrue.user");
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
