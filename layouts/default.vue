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
				@click="dialog = true"
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
					@click="dialog = true"
					fab
					dark
					color="indigo"
					fixed
					right="20"
					bottom="20"
				>
					<v-icon dark>mdi-plus</v-icon>
				</v-btn>
				<v-dialog v-model="dialog" width="90vw" max-width="1024px">
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
};
</script>
