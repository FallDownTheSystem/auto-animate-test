<script setup>
let array = $ref([...Array(100).keys()]);
let shuffle = e => {
	// Randomize elements in array
	for (let i = array.length - 1; i > 0; i--) {
		let j = Math.floor(Math.random() * (i + 1));
		[array[i], array[j]] = [array[j], array[i]];
	}
};

let index = 101;
let add = e => {
	// Add element to array
	array.push(index++);
};

let remove = e => {
	// Remove random element from array
	array.splice(Math.floor(Math.random() * array.length), 1);
};

let reset = e => {
	array = [...Array(100).keys()];
};
</script>

<template>
	<p>Left TransitionGroup, Right v-auto-animate</p>

	<button @click="shuffle">Shuffle</button>
	<button @click="add">Add</button>
	<button @click="remove">Remove</button>
	<button @click="reset">Reset</button>

	<div class="grids">
		<TransitionGroup tag="main" name="fade" style="margin-right: 32px">
			<div v-for="i in array" :key="i">
				{{ i }}
			</div>
		</TransitionGroup>

		<main v-auto-animate="{ duration: 750 }">
			<div v-for="i in array" :key="i">
				{{ i }}
			</div>
		</main>
	</div>
</template>

<style>
html {
	height: 100%;
	font-family: sans-serif;
}

body {
	background-color: black;
	color: white;
	display: flex;
	height: 100%;
	align-items: center;
	justify-content: center;
	overflow: hidden;
}

.grids {
	display: flex;
	height: 500px;
	width: 800px;
	align-items: center;
	justify-content: center;
	border: 1px dashed gray;
}

main {
	position: relative;
	display: grid;
	grid-template-columns: repeat(10, 1fr);
	grid-template-rows: repeat(10, 1fr);
	justify-content: space-around;
}
main > div {
	display: flex;
	width: 32px;
	height: 32px;
	align-items: center;
	justify-content: center;
	box-sizing: border-box;
	border: 1px solid gray;
}

/* 1. declare transition */
.fade-move,
.fade-enter-active,
.fade-leave-active {
	transition: all 0.75s cubic-bezier(0.55, 0, 0.1, 1);
}

/* 2. declare enter from and leave to state */
.fade-enter-from,
.fade-leave-to {
	opacity: 0;
	transform: scaleY(0.01) translate(30px, 0);
}

/* 3. ensure leaving items are taken out of layout flow so that moving
      animations can be calculated correctly. */
.fade-leave-active {
	position: absolute;
}
</style>
