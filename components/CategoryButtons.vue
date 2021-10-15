<template>
	<v-row dense>
		<v-col v-for="c in categories" :key="c.category" cols="6" sm="4" md="3">
			<v-card
				style="text-align: center"
				:to="c.category ? 'recipes?category=' + c.category : 'recipes'"
			>
				<v-icon x-large class="mt-4 mb-n2">mdi-{{ c.icon }}</v-icon>
				<v-card-text class="caption">{{ c.label }}</v-card-text>
			</v-card>
		</v-col>
	</v-row>
</template>

<script>
export default {
	data: () => ({
		categories: [
			{
				category: "bread",
				label: "Bread",
				icon: "baguette",
			},
			{
				category: "breakfast",
				label: "Breakfast & Brunch",
				icon: "egg",
			},
			{
				category: "dessert",
				label: "Dessert",
				icon: "cupcake",
			},
			{
				category: "main",
				label: "Main Dish",
				icon: "silverware-fork-knife",
			},
			{
				category: "side",
				label: "Side Dish",
				icon: "noodles",
			},
			{
				category: "snack",
				label: "Snack or Appetizer",
				icon: "pretzel",
			},
			{
				category: "soup",
				label: "Soup, Stew & Chili",
				icon: "bowl-mix",
			},
			{
				category: "",
				label: "Search",
				icon: "magnify",
			},
		],
	}),
};
</script>
