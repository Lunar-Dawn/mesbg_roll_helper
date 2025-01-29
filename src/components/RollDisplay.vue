<script setup lang="ts">
import Dice1 from '../assets/Dice-1-b.svg'
import Dice2 from '../assets/Dice-2-b.svg'
import Dice3 from '../assets/Dice-3-b.svg'
import Dice4 from '../assets/Dice-4-b.svg'
import Dice5 from '../assets/Dice-5-b.svg'
import Dice6 from '../assets/Dice-6-b.svg'

const images = [
	Dice1,
	Dice2,
	Dice3,
	Dice4,
	Dice5,
	Dice6,
]

defineProps<{
	dice: [number, number],
	correct: boolean,
}>()
</script>

<template>
<div class="dice" :class="{ 'correct': correct }">
	<img class="dice-img" :src="images[dice[0] - 1]"/>
	<img class="dice-img" :src="images[dice[1] - 1]"/>
	<div class="sum">
		{{ dice[0] + dice[1] }}
	</div>
</div>
</template>

<style scoped lang="scss">
.dice {
	display: grid;
	grid-template-columns: 1fr 1fr;

	background: darkred;
	padding: 10px;
	border-radius: 5px;

	img {
		height: var(--row-height);

		filter: invert(1);

		&:first-child {
			justify-self: end;
		}
	}

	.sum {
		grid-column: span 2;
		text-align: center;
		font-weight: bold;
	}
}

.correct {
	background: green;
}
</style>