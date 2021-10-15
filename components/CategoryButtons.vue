<template>
	<v-row dense>
		<v-col v-for="c in categories" :key="c.category" cols="6" sm="4" md="3">
			<v-card
				style="text-align: center"
				:to="
					c.category === 'search' ? 'recipes' : 'recipes?category=' + c.category
				"
			>
				<img
					:src="require(`~/assets/icons/${c.category}.svg`)"
					class="mt-4 mb-n4"
					height="64px"
					width="64px"
				/>
				<v-card-text class="caption"><b>{{ c.label }}</b></v-card-text>
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
			},
			{
				category: "breakfast",
				label: "Breakfast & Brunch",
			},
			{
				category: "dessert",
				label: "Dessert",
			},
			{
				category: "main",
				label: "Main Dish",
			},
			{
				category: "side",
				label: "Side Dish",
			},
			{
				category: "snack",
				label: "Snack or Appetizer",
			},
			{
				category: "soup",
				label: "Soup, Stew & Chili",
			},
			{
				category: "search",
				label: "Search",
			},
		],
	}),
};
</script>
