<template>
	<div class="natural-typography">
		<div v-if="$vuetify.breakpoint.mdAndDown">
			<h1 class="mb-0">{{ recipe.title }}</h1>
			<p>{{ recipe.description }}</p>
			<p v-if="recipe.author">
				Added by: <b>{{ recipe.author }}</b>
			</p>
		</div>
		<v-row :class="$vuetify.breakpoint.lgAndUp ? 'mt-8' : ''">
			<v-col cols="12" lg="5">
				<v-card :href="recipe.url" style="overflow: hidden">
					<v-img v-if="recipe.image" :src="recipe.image" aspect-ratio="1.333" />
					<v-list-item v-if="recipe.url">
						<v-list-item-avatar v-if="recipe.image">
							<v-img :src="recipe.image"></v-img>
						</v-list-item-avatar>
						<v-list-item-content v-if="recipe.title && recipe.description">
							<v-list-item-title v-text="recipe.title" />
							<v-list-item-subtitle v-text="recipe.description" />
						</v-list-item-content>
						<v-list-item-action>
							<v-btn icon>
								<v-icon color="indigo">mdi-launch</v-icon>
							</v-btn>
						</v-list-item-action>
					</v-list-item>
				</v-card>
			</v-col>
			<v-col cols="12" lg="7">
				<div v-if="$vuetify.breakpoint.lgAndUp">
					<h1 class="mb-0">{{ recipe.title }}</h1>
					<p>{{ recipe.description }}</p>
					<p v-if="recipe.author">
						Added by: <b>{{ recipe.author }}</b>
					</p>
				</div>
				<nuxt-content :document="recipe" />
			</v-col>
		</v-row>
	</div>
</template>

<script>
export default {
	async asyncData({ $content, params, error }) {
		let recipe;

		try {
			recipe = await $content("recipes", { deep: true })
				.where({ path: { $eq: "/recipes/" + params.pathMatch } })
				.fetch();
		} catch (e) {
			error({ message: "Recipe not found" });
		}

		recipe = recipe[0];

		return {
			recipe,
		};
	},
};
</script>
