<template>
	<div class="natural-typography">
		<h1>Find a Recipe</h1>
		<v-row class="mb-8">
			<v-col cols="12" md="4">
				<v-text-field
					v-model="query"
					placeholder="Search"
					prepend-inner-icon="mdi-magnify"
					hide-details
					rounded
					solo
					color="accent"
					@change="search"
				/>
			</v-col>
			<v-col cols="12" md="4">
				<v-select
					v-model="category"
					:items="categories"
					hide-details
					rounded
					solo
					item-color="accent"
					@change="search"
				/>
			</v-col>
			<v-col cols="12" md="4">
				<v-select
					v-model="author"
					:items="authors"
					hide-details
					rounded
					solo
					item-color="accent"
					@change="search"
				/>
			</v-col>
			<v-col v-if="(query || category || author) && searchResults.length">
				<v-btn color="primary" dark rounded @click="resetFilters">
					Reset Filters
				</v-btn>
			</v-col>
		</v-row>
		<div v-if="!searchResults.length">
			<h2>No results found.</h2>
			<p>Try a different search or reset your filters.</p>
			<v-btn color="primary" dark rounded @click="resetFilters">
				Reset Filters
			</v-btn>
		</div>
		<RecipeList v-else :recipes="searchResults" ref="list" />
	</div>
</template>

<script>
export default {
	data: () => ({
		searchResults: [],
		categories: [
			{ text: "Choose a Category", value: "" },
			{ text: "Bread", value: "bread" },
			{ text: "Breakfast or Brunch", value: "breakfast" },
			{ text: "Dessert", value: "dessert" },
			{ text: "Main Dish", value: "main" },
			{ text: "Side Dish", value: "side" },
			{ text: "Snack or Appetizer", value: "snack" },
			{ text: "Soup, Stew or Chili", value: "soup" },
		],
		authors: [{ text: "Choose an Author", value: "" }],
		category: "",
		author: "",
		query: "",
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
		return {
			recipes,
		};
	},
	head() {
		return {
			title: "Search",
		};
	},
	methods: {
		search() {
			let matches = this.recipes;
			if (this.query) {
				matches = matches.filter(
					(recipe) =>
						recipe.title.toLowerCase().includes(this.query.toLowerCase()) ||
						JSON.stringify(recipe.body)
							.toLowerCase()
							.includes(this.query.toLowerCase())
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
			this.$refs.list && this.$refs.list.updateLayout();
			setTimeout(() => {
				this.$refs.list && this.$refs.list.updateLayout();
			}, 300);
		},
		setAuthors() {
			let authors = [];
			this.recipes.forEach((recipe) => {
				const author = recipe.author;
				if (!authors.includes(author)) authors.push(author);
			});
			authors = authors.sort();
			this.authors.push(...authors);
		},
		resetFilters() {
			this.query = "";
			this.category = "";
			this.author = "";
			this.search();
		},
	},
	created() {
		const category = this.$route.query.category;
		if (category) this.category = category;
		this.search();
		this.setAuthors();
	},
};
</script>
