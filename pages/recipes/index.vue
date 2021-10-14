<template>
	<div class="natural-typography">
		<h1>Find a Recipe</h1>
		<v-row :dense="$vuetify.breakpoint.mdAndUp ? false : true" class="mb-8">
			<v-col cols="12" md="4">
				<v-text-field
					v-model="query"
					placeholder="Search"
					hide-details
					@change="search"
				/>
			</v-col>
			<v-col cols="12" md="4">
				<v-select
					v-model="category"
					:items="recipes.categories"
					placeholder="Category"
					hide-details
					@change="search"
				/>
			</v-col>
			<v-col cols="12" md="4">
				<v-select
					v-model="author"
					:items="recipes.authors"
					placeholder="Author"
					hide-details
					@change="search"
				/>
			</v-col>
		</v-row>
		<RecipeList :recipes="searchResults.length > 0 ? searchResults : recipes" />
	</div>
</template>

<script>
export default {
	data: () => ({
		searchResults: [],
		query: "",
		category: "",
		author: "",
	}),
	async asyncData({ $content, params, error }) {
		let recipes;
		try {
			recipes = await $content("recipes", { deep: true })
				.sortBy("createdAt", "desc")
				.fetch();
		} catch (e) {
			error({ message: "Recipes not found" });
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
	methods: {
		search() {
			let matches = this.recipes;
			console.log(matches);
			if (this.query) {
				matches = matches.filter(
					(recipe) =>
						recipe.title.includes(this.query) ||
						JSON.stringify(recipe.body).includes(this.query)
				);
			}
			if (this.category) {
				matches = matches.filter((recipe) =>
					recipe.path.startsWith("/recipes/" + this.category)
				);
			}
			if (this.author) {
				matches = matches.filter((recipe) => recipe.author === this.author);
			}
			this.searchResults = matches;
		},
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
