<template>
	<v-app>
		<v-app-bar app color="green lighten-1" dark>
			<v-spacer></v-spacer>
			<v-toolbar-title>Cooldowns Tracks0r</v-toolbar-title>
			<v-spacer></v-spacer>
		</v-app-bar>

		<v-main id="app_main">
			<Layout />
		</v-main>
	</v-app>
</template>

<script>
import Layout from "./components/Layout.vue";
export default {
	name: "App",

	components: { Layout },

	data: () => ({
		//
	}),
};
</script>

<style>
#app_main {
	background-color: #111111;
}
body.modal-open {
	overflow: hidden;
	height: 100vh;
}
::-webkit-scrollbar {
	display: none;
}
</style>
