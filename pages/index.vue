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
		<masonry-layout gap="16px">
			<v-card
				v-for="recipe in recipes"
				:key="recipe.slug"
				:to="recipe.path"
				outlined
			>
				<v-img :src="recipe.image" aspect-ratio="1.333" />
				<v-card-title>
					<h3 class="title">{{ recipe.title }}</h3>
				</v-card-title>
				<v-card-text>
					<p class="my-0">{{ recipe.description }}</p>
				</v-card-text>
			</v-card>
		</masonry-layout>
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
