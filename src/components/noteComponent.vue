<script setup lang="ts">
	interface propsInterface {
		content: string;
		clickEvent: (payload: MouseEvent) => void;
	}
	defineProps<propsInterface>();
</script>

<template>
	<li class="note" @click="clickEvent">
		{{ content }}
	</li>
</template>
<style scoped>
	.note {
		padding: 10px 5px;
		width: 170px;
		height: 200px;
		margin: 10px 15px;
		background-color: pink;
		border-radius: 10px;
		color: black;
		font-size: 18px;
		transition: 20ms ease-out;
		cursor: pointer;
		word-wrap: break-word;
		overflow: hidden;
	}

	.note:hover {
		transform: scale(1.1);
		transition: 40ms ease-in;
	}
</style>
