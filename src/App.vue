<script setup lang="ts">
	import noteListComponent from './components/noteListComponent.vue';
</script>

<template>
	<h1>Simple Notes App</h1>
	<noteListComponent />
</template>

<style>
	body {
		background-color: rgb(31, 31, 31);
		color: white;
		box-sizing: border-box;
	}

	#app {
		width: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	li {
		list-style: none;
	}
</style>
