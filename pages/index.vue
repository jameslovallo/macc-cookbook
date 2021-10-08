<template>
	<div class="natural-typography">
		<div>
			<h1>Welcome to the Mack Avenue Community Church Cookbook!</h1>
			<p>
				This is the place to share all of your favorite recipes with the rest of
				the church.
			</p>
			<h2>Recently Added</h2>
		</div>
		<RecipeList :recipes="recipes" />
	</div>
</template>

<script>
export default {
	async asyncData({ $content, params, error }) {
		let recipes;
		try {
			recipes = await $content("recipes").sortBy("createdAt", "desc").fetch();
		} catch (e) {
			error({ message: "Blog Post not found" });
		}

		return {
			recipes,
		};
	},
};
</script>
