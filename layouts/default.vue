<template>
	<v-app>
		<v-app-bar app>
			<nuxt-link to="/" style="color: inherit; text-decoration: none">
				<v-toolbar-title v-text="title" />
			</nuxt-link>
			<v-spacer />
			<v-btn to="/recipes" text>Recipes</v-btn>
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
				<v-btn
					v-if="$vuetify.breakpoint.smAndDown"
					@click="signin"
					fab
					dark
					color="indigo"
					fixed
					:right="true"
					:bottom="true"
				>
					<v-icon dark>mdi-plus</v-icon>
				</v-btn>
				<v-dialog v-model="dialog" width="95vw" max-width="1024px">
					<Admin />
				</v-dialog>
			</v-container>
		</v-main>
	</v-app>
</template>

<script>
export default {
	data: () => ({
		dialog: false,
		signedIn: false,
		title: "MACC Cookbook",
	}),
	methods: {
		signin() {
			this.signedIn === true || window.location.hostname === "localhost"
				? (this.dialog = true)
				: netlifyIdentity.open();
		},
	},
};
</script>

<style>
	.v-dialog {
		margin: 0 !important;
	}
</style>
