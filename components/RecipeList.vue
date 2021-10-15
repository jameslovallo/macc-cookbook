<template>
	<masonry-layout :gap="gap" ref="masonry">
		<v-card
			v-for="r in recipes"
			:key="r.slug"
			:to="r.path"
			:style="{ marginBottom: gap }"
		>
			<img :src="r.image" />
			<v-card-title>
				<h3 class="title">{{ r.title }}</h3>
			</v-card-title>
			<v-card-text>
				<p v-if="r.description" class="my-0">{{ r.description }}</p>
				<p v-if="r.author" class="mb-0">
					Added by: <b>{{ r.author }}</b>
				</p>
			</v-card-text>
		</v-card>
	</masonry-layout>
</template>

<script>
export default {
	props: ["recipes"],
	computed: {
		gap() {
			return this.$vuetify.breakpoint.mdAndUp ? "24px" : "16px";
		},
	},
	methods: {
		updateLayout() {
			this.$refs.masonry.layout();
		},
	},
};
</script>
