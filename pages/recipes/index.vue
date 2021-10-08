<template>
	<div class="natural-typography">
		<h1>Recipes</h1>
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

<style lang="scss">
	.title {
		word-break: keep-all;
	}
	masonry-layout {
		.v-card {
			margin-bottom: 16px;
			&__text {
				width: auto;
			}
		}
	}
</style>
