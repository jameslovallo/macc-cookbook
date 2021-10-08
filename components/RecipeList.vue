<template>
	<masonry-layout gap="16px">
		<v-card
			v-for="recipe in recipes"
			:key="recipe.slug"
			:to="recipe.path"
			outlined
		>
			<img :src="recipe.image" />
			<v-card-title>
				<h3 class="title">{{ recipe.title }}</h3>
			</v-card-title>
			<v-card-text>
				<p class="mb-0">{{ recipe.description }}</p>
			</v-card-text>
		</v-card>
	</masonry-layout>
</template>

<script>
export default {
	props: ["recipes"],
};
</script>
