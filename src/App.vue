<script setup lang="ts">
import RollOption from "./components/RollOption.vue"
import {computed, ref} from "vue"
import RollResult from "./components/RollResult.vue"
import type { Result } from "./result.ts"

const results = ref<Record<number, Result>>({})
for (let i = 0; i < 11; i++) {
	results.value[i + 2] = {
		index: i + 2,
		active: false,
		used: false,
	}
}

const delay = ref(100)

const rolling = ref(false)
const found1 = ref(false)
const tries1 = ref<[number, number][]>([])
const found2 = ref(false)
const tries2 = ref<[number, number][]>([])

const num_enabled = computed(() => Object.values(results.value).filter(r => r.active).length)

const dice = (): [number, number] => {
	return [(Math.floor(Math.random() * 6) + 1), (Math.floor(Math.random() * 6) + 1)]
}
const tryPickValue = (tries: [number, number][]): boolean => {
	tries.push(dice())
	const [r1, r2] = tries[tries.length - 1]
	const res = results.value[r1 + r2]

	if(res.active && !res.used) {
		res.used = true
		return true
	}

	return false
}
const tryFindValues = () => {
	if(!found1.value)
		found1.value = tryPickValue(tries1.value)

	if(!found2.value)
		found2.value = tryPickValue(tries2.value)
}

const startRoll = () => {
	if(num_enabled.value < 2 || rolling.value)
		return

	rolling.value = true

	found1.value = false
	tries1.value = []
	found2.value = false
	tries2.value = []

	Object.values(results.value).forEach(r => r.used = false)

	if(!delay.value) {
		while(!found1.value || !found2.value) {
			tryFindValues()
		}

		rolling.value = false
	} else {
		const intervalID = setInterval(() => {
			tryFindValues()

			if(found1.value && found2.value) {
				clearInterval(intervalID)
				rolling.value = false
			}
		}, delay.value)
	}
}
</script>

<template>
<main class="body-wrapper">
	<h1>MESBG Level-up roller</h1>
	<div class="selector-table">
		<RollOption v-for="(res, i) in results" v-model="results[i]" :key="res.index"/>
	</div>
	<div class="button-wrapper">
		<button @click="startRoll" v-if="num_enabled >= 2">
			{{ rolling ? 'Rolling...' : 'Roll' }}
		</button>
		<span v-else>Please select at least 2 options</span>
	</div>
	<RollResult :progression="tries1" :done="found1" :display="rolling || found1 || found2"/>
	<RollResult :progression="tries2" :done="found2" :display="rolling || found1 || found2"/>
</main>
</template>

<style scoped lang="scss">
.body-wrapper {
	max-width: 100vw;

	display: grid;
	grid-template-columns: 1fr 1fr;
	gap: 0 5px;
}
h1 {
	grid-column: 1/-1;

	justify-self: center;
}
.selector-table {
	grid-column: 1/-1;
	display: grid;
	grid-template-columns: repeat(11, 1fr);
	grid-template-rows: repeat(7, 1fr);

	align-items: stretch;
}
.button-wrapper {
	grid-column: 1/-1;

	display: flex;

	height: var(--row-height);

	span {
		align-self: center;

		width: 100%;
		text-align: center;
	}

	button {
		width: 100%;

		font-size: 1em;

		border: 0;
	}
}
</style>
