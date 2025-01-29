<script setup lang="ts">
import type {Result} from "../result.ts";

const res = defineModel<Result>({ required: true })

const toggle = () => {
	res.value.active = !res.value.active
}
</script>

<template>
<div class="option" @click="toggle" :class="{ active: res.active, used: res.active && res.used }">
	<div class="probability-bar" :style="{ '--offset': Math.abs(7 - res.index) + 1 }"></div>

	<div class="number">
		{{ res.index }}
	</div>
</div>
</template>

<style scoped lang="scss">
.option {
	grid-row: 1/-1;

	display: grid;
	grid-template-rows: subgrid;

	color: grey;

	padding: 3px;

	.probability-bar {
		grid-row: var(--offset) / -2;

		background: darkred;

		border-radius: 4px 4px 0 0;
	}

	.number {
		grid-row-end: -1;
		justify-self: center;
		font-weight: bold;
	}
}
.active {
	color: white;

	.probability-bar {
		background: forestgreen;
	}
}

.used .probability-bar {
	background: goldenrod;
}
</style>