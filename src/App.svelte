<script lang="ts">
import svelteLogo from "./assets/svelte.svg";
import Counter from "./lib/Counter.svelte";
import { flip } from "svelte/animate";
import { bubble } from "svelte/internal";
const idea: string = "Sorting Algorithms";
let speed = 25;

var edition = popul([]);
var arrBubble = [];
edition.forEach(val => arrBubble.push(Object.assign({}, val)));
var arrMerge = []; 
edition.forEach(val => arrMerge.push(Object.assign({}, val)));
var arrGravity = []; 
edition.forEach(val => arrGravity.push(Object.assign({}, val)));
var count = 1;

function popul(
		arr: { state: string; num: number }[]
	      ): { state: string; num: number }[] {
	for (let i = 0; i < 40; i++) {
		let example = {
state: "whitesmoke",
       num: Math.floor(Math.random() * 50 + 1),
		};
		arr.push(example);
	}
	return arr;
}

function sortBubble(delay: number) {
	var i = 0;
	var j = 0;

	function task(delay: number) {
		count++;
		if (j >= arrBubble.length - i - 1) {
			arrBubble[j].state = "#8fbc8f";
			j = 0;
			i++;
		}

		if (i < arrBubble.length) {
			if (arrBubble[j].num > arrBubble[j + 1].num) {
				var temp = arrBubble[j];
				arrBubble[j] = arrBubble[j + 1];
				arrBubble[j + 1] = temp;
			} else {
				arrBubble[j].state = "whitesmoke";
				arrBubble[j + 1].state = "crimson";
			}
			j++;
			console.log(delay)
			setTimeout(task, delay);
		} else {
			arrBubble[0].state = "#8fbc8f";
		}
	}
	task(delay);
}

async function mergeSortPointers(arr: any[], start: number, end: number, delay: number): Promise<void> {
	if (start >= end) return;

	const middle = Math.floor((start + end) / 2);

	await mergeSortPointers(arr, start, middle, delay);
	await mergeSortPointers(arr, middle + 1, end, delay);

	const temp = [];
	let leftIndex = start;
	let rightIndex = middle + 1;

	for (let i = start; i <= end; i++) {
	
		await new Promise(resolve => setTimeout(resolve, delay));
		if (leftIndex <= middle && (rightIndex > end || arr[leftIndex].num < arr[rightIndex].num)) {
		
			temp[i] = arr[leftIndex];
			leftIndex++;
		} else {
		
			temp[i] = arr[rightIndex];
			rightIndex++;
		}
	}

	for (let i = start; i <= end; i++) {
		arr[i] = temp[i];
		arr[i].state = "#8fbc8f"
	}	
	arrMerge = arrMerge
}

async function gravitySortObjects(arr: any[], delay: number): Promise<void> {
	const max = Math.max(...arr.map(item => item.num));
	const min = Math.min(...arr.map(item => item.num));
	const range = max - min + 1;
	const count = Array(range).fill(0);

	for (let i = 0; i < arr.length; i++) {
		count[arr[i].num - min]++;
	}

	let index = 0;
	for (let i = 0; i < range; i++) {
		await new Promise(resolve => setTimeout(resolve, delay));
		for (let j = 0; j < count[i]; j++) {
			arr[index++] = { state: '#8fbc8f', num: i + min };
			arrGravity = arrGravity
		}
	}
}

function mergeSort() {
	mergeSortPointers(arrMerge, 0, arrMerge.length - 1, (50 - speed) / 1.8)
}
function gravitySort() {
	gravitySortObjects(arrGravity, 50 - speed)
}
function bubbleSort() {
	sortBubble(1000)
}
</script>

<main>
<p> {speed} </p>
<div>
  <input type="range" min="1" max="50" id="delay" bind:value={speed}>
</div>
  <h1>{idea}</h1>
  <div class="card" style="display: inline-block">
    <div>
      {#each arrBubble as num}
        <div
          style="height: {num.num}px;width: 10px;background-color: {num.state};display:
          inline-block;"
        />
      {/each}
    </div>
    <div class="card">
      <button on:click={bubbleSort}>Bubble Sort</button>
    </div>
  </div>
  <div class="card" style="display: inline-block">
    <div>
      {#each arrMerge as num}
        <div
          style="height: {num.num}px;width: 10px;background-color: {num.state};display:
          inline-block;"
        />
      {/each}
    </div>
    <div class="card">
      <button on:click={mergeSort}>Merge Sort</button>
    </div>
  </div>
  <div class="card" style="display: inline-block">
    <div>
      {#each arrGravity as num}
        <div
          style="height: {num.num}px;width: 10px;background-color: {num.state};display:
          inline-block;"
        />
      {/each}
    </div>
    <div class="card">
      <button on:click={gravitySort}>Gravity Sort</button>
    </div>
  </div>

  <p>Powered by Svelte and Vite</p>
</main>

<style>
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  .logo.svelte:hover {
    filter: drop-shadow(0 0 2em #ff3e00aa);
  }
  .read-the-docs {
    color: #888;
  }
  .square {
    width: 10px;
    background-color: whitesmoke;
    display: inline-block;
  }
</style>
