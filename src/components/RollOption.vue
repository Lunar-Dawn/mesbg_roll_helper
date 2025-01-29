<script setup lang="ts">
import type {Result} from "../result.ts";

const res = defineModel<Result>({ required: true })

const toggle = () => {
	res.value.active = !res.value.active
}
</script>

<template>
<div class="row" @click="toggle" :class="{ active: res.active }">
	<div class="bg-wrapper">
		<span>Row {{ res.index }}</span>
	</div>
</div>
</template>

<style scoped lang="scss">
.row {
	color: grey;

	height: var(--row-height);

	&:nth-of-type(even) > .bg-wrapper {
		background: rgb(192 192 192 / 20%);
	}

	.bg-wrapper {
		width: 100%;
		height: 100%;

		display: flex;
		justify-content: center;
		align-items: center;
	}
}
.active {
	background: darkgreen;
	color: white
}
</style>