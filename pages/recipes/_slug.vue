<template>
	<div class="natural-typography">
		<h1>{{ post.title }}</h1>
		<v-row>
			<v-col cols="12" lg="6">
				<v-img :src="post.image" aspect-ratio="1.333"></v-img>
			</v-col>
			<v-col cols="12" lg="6">
				<nuxt-content :document="post" />
			</v-col>
		</v-row>
	</div>
</template>

<script>
export default {
	async asyncData({ $content, params, error }) {
		let post;
		try {
			post = await $content("recipes", params.slug).fetch();
			// OR const article = await $content(`articles/${params.slug}`).fetch()
		} catch (e) {
			error({ message: "Blog Post not found" });
		}

		return {
			post,
		};
	},
};
</script>
