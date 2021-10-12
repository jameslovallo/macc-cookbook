<template>
	<div class="natural-typography">
		<h1>Find a Recipe</h1>
		<v-row :dense="$vuetify.breakpoint.mdAndUp ? false : true" class="mb-8">
			<v-col cols="12" md="4">
				<v-text-field placeholder="Search" hide-details></v-text-field>
			</v-col>
			<v-col cols="12" md="4">
				<v-select
					:items="recipes.categories"
					placeholder="Category"
					hide-details
				></v-select>
			</v-col>
			<v-col cols="12" md="4">
				<v-select
					:items="recipes.authors"
					placeholder="Author"
					hide-details
				></v-select>
			</v-col>
		</v-row>
		<RecipeList :recipes="recipes" />
	</div>
</template>

<script>
export default {
	async asyncData({ $content, params, error }) {
		let recipes;
		try {
			recipes = await $content("recipes", { deep: true })
				.sortBy("createdAt", "desc")
				.fetch();
		} catch (e) {
			error({ message: "Blog Post not found" });
		}

		const categories = () => {
			let cats = [];
			recipes.forEach((recipe) => {
				let cat = recipe.dir.split("/")[2];
				if (!cats.includes(cat)) cats.push(cat);
			});
			cats = cats.sort();
			recipes.categories = cats;
		};
		categories();

		const authors = () => {
			let authors = [];
			recipes.forEach((recipe) => {
				let author = recipe.author;
				if (!authors.includes(author)) authors.push(author);
			});
			authors = authors.sort();
			recipes.authors = authors;
		};
		authors();

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
