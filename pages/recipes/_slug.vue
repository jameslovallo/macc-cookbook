<template>
	<div class="natural-typography">
		<h1>{{ recipe.title }}</h1>
		<v-row>
			<v-col cols="12" lg="6">
				<v-img :src="recipe.image" aspect-ratio="1.333"></v-img>
			</v-col>
			<v-col cols="12" lg="6">
				<nuxt-content :document="recipe" />
			</v-col>
		</v-row>
		{{ recipe }}
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

		return {
			recipe,
		};
	},
};
</script>
