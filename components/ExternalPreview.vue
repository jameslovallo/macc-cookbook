<template>
	<v-card :href="recipe.url" style="overflow: hidden">
		<v-img v-if="recipe.image" :src="recipe.image" aspect-ratio="1.333" />
		<v-list-item v-if="recipe.url">
			<v-list-item-avatar v-if="recipe.image">
				<v-img :src="recipe.image"></v-img>
			</v-list-item-avatar>
			<v-list-item-content v-if="recipe.title && recipe.description">
				<v-list-item-title v-text="recipe.title" />
				<v-list-item-subtitle v-text="recipe.description" />
			</v-list-item-content>
			<v-list-item-action>
				<v-btn icon>
					<v-icon color="indigo">mdi-launch</v-icon>
				</v-btn>
			</v-list-item-action>
		</v-list-item>
	</v-card>
</template>

<script>
export default {
	props: ["recipe"],
};
</script>
