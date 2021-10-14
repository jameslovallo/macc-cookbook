<template>
	<div class="natural-typography">
		<div>
			<h1 :style="{ fontSize: $vuetify.breakpoint.lgAndUp ? '4em' : '' }">
				Welcome to the Mack Avenue Community Church Cookbook!
			</h1>
			<p>
				This is the place to share recipes with your MACC Fam and discover new
				favorites.
			</p>
			<h2 :style="{ fontSize: $vuetify.breakpoint.lgAndUp ? '2em' : '' }">
				Recently Added
			</h2>
		</div>
		<RecipeList :recipes="recipes" />
	</div>
</template>

<script>
export default {
	async asyncData({ $content, params, error }) {
		let recipes;
		try {
			recipes = await $content("recipes", { deep: true })
				.limit(10)
				.sortBy("createdAt", "desc")
				.fetch();
		} catch (e) {
			error({ message: "Blog Post not found" });
		}

		return {
			recipes,
		};
	},
};
</script>
