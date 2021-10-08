<template>
	<masonry-layout gap="16px">
		<v-card v-for="recipe in recipes" :key="recipe.slug" :to="recipe.path">
			<img :src="recipe.image" />
			<v-card-title>
				<h3 class="title">{{ recipe.title }}</h3>
			</v-card-title>
			<v-card-text>
				<p v-if="recipe.description" class="my-0">{{ recipe.description }}</p>
				<p v-if="recipe.author" class="mb-0">Added by: {{ recipe.author }}</p>
			</v-card-text>
		</v-card>
	</masonry-layout>
</template>

<script>
export default {
	props: ["recipes"],
};
</script>
