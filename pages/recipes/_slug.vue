<template>
	<div class="natural-typography">
		<h1 class="mb-0">{{ recipe.title }}</h1>
		<p v-if="recipe.author">
			Added by <b>{{ recipe.author }}</b>
		</p>
		<v-row>
			<v-col cols="12" lg="5">
				<v-card
					:href="recipe.siteMeta.url"
					flat
					outlined
					style="overflow: hidden"
				>
					<v-img
						v-if="recipe.image || recipe.siteMeta.image"
						:src="recipe.image || recipe.siteMeta.image"
						aspect-ratio="1.333"
					/>
					<v-list-item>
						<v-list-item-avatar v-if="recipe.siteMeta.image">
							<v-img :src="recipe.siteMeta.image"></v-img>
						</v-list-item-avatar>
						<v-list-item-content
							v-if="recipe.siteMeta.title && recipe.siteMeta.description"
						>
							<v-list-item-title v-text="recipe.siteMeta.title" />
							<v-list-item-subtitle v-text="recipe.siteMeta.description" />
						</v-list-item-content>
					</v-list-item>
				</v-card>
			</v-col>
			<v-col cols="12" lg="7">
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
			recipe = await $content("recipes", params.slug).fetch();
			// OR const article = await $content(`articles/${params.slug}`).fetch()
		} catch (e) {
			error({ message: "Blog Post not found" });
		}

		if (recipe.url) {
			await fetch(
				`http://api.linkpreview.net/?key=a7809f8c76c2dea3ab3ea3be91b02b60&q=${recipe.url}`
			)
				.then((response) => response.json())
				.then((data) => (recipe.siteMeta = data));
		}

		return {
			recipe,
		};
	},
};
</script>
