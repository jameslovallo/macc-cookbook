<template>
	<div class="natural-typography">
		<div v-if="$vuetify.breakpoint.mdAndDown">
			<h1 class="mb-0">{{ recipe.title }}</h1>
			<p>{{ recipe.description }}</p>
			<p v-if="recipe.author">
				Added by: <b>{{ recipe.author }}</b>
			</p>
		</div>
		<v-row :class="$vuetify.breakpoint.lgAndUp ? 'mt-8' : ''">
			<v-col cols="12" md="5">
				<ExternalPreview :recipe="recipe" />
			</v-col>
			<v-col cols="12" md="7">
				<div v-if="$vuetify.breakpoint.lgAndUp">
					<h1 class="mb-0">{{ recipe.title }}</h1>
					<p>{{ recipe.description }}</p>
					<p v-if="recipe.author">
						Added by: <b>{{ recipe.author }}</b>
					</p>
				</div>
				<nuxt-content :document="recipe" />
				<h2>Comments</h2>
				<v-card>
					<v-card-text>
						<div id="disqus_thread"></div>
					</v-card-text>
				</v-card>
			</v-col>
		</v-row>
	</div>
</template>

<script>
export default {
	async asyncData({ $content, params, error }) {
		let recipe;

		try {
			recipe = await $content("recipes", { deep: true })
				.where({ path: { $eq: "/recipes/" + params.pathMatch } })
				.fetch();
		} catch (e) {
			error({ message: "Recipe not found" });
		}

		recipe = recipe[0];

		return {
			recipe,
		};
	},
	head() {
		return {
			title: this.recipe.title,
		};
	},
	methods: {
		disqus() {
			var d = document,
				s = d.createElement("script");
			s.src = "https://macc-cookbook.disqus.com/embed.js";
			s.setAttribute("data-timestamp", +new Date());
			(d.head || d.body).appendChild(s);
		},
	},
	mounted() {
		this.disqus();
	},
};
</script>
