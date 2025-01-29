<script setup lang="ts">
import {computed} from "vue"
import RollDisplay from "./RollDisplay.vue";

const props = defineProps<{
	progression: [number, number][],
	done: boolean,
	display: boolean,
}>()

const last = computed(() => props.progression[props.progression.length - 1])
</script>

<template>
<div class="wrapper">
	<h1>
		<template v-if="display">
			<template v-if="done">
				{{ last[0] + last[1] }}
			</template>
			<template v-else>
				Rolling...
			</template>
		</template>
	</h1>
	<div class="dice-wrapper">
		<RollDisplay v-if="display" v-for="(dice, index) in progression.slice().reverse()" :correct="index === 0 && done" :dice="dice" :key="index"/>
	</div>
</div>
</template>

<style scoped lang="scss">
.wrapper {
	display: flex;
	flex-direction: column;
	align-items: center;
}
.complete-result {
	font-weight: bold;
}

.dice-wrapper {
	width: 100%;

	display: grid;
	grid-template-columns: repeat(3, calc(var(--row-height) * 2 + 20px));
	justify-content: center;
	gap: 5px;
}

ul {
	margin: 0;
	padding: 0;
}
li {
	list-style: none;
}
</style>