<script>
import { onMount } from "svelte";


let field = []

const width = 15
const fieldLength = 60
let speed = 4

for (let i = 0; i < fieldLength; i++) {
	field[i] = []
	for (let j = 0; j < fieldLength; j++) {
		field[i][j] = j
	}
}


class Snake {
	constructor() {
		this.coords = [[getRandomInt(10, 20), getRandomInt(10, 20)]]
		this.snakeSize = 1;
		this.directionNumb = getRandomInt(0, 3)
		this.direction = this.directionNumb === 0 ? 'left' : this.directionNumb === 1 ? 'top' :  this.directionNumb === 2 ? 'right' :  'bottom'
	}

	sizeUp () {
		// this.coords.push(this.coords[this.coords.length - 1])
		this.coords = [...this.coords, this.coords[this.coords.length - 1]]
		this.snakeSize++
	}

}

const snake = new Snake()






function getRandomInt(min, max) {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.floor(Math.random() * (max - min)) + min; //Максимум не включается, минимум включается
}



console.log(snake.coords[0], snake.direction)



const handleKeydown = (e) => {
	console.log(e.key)
	switch (e.key) {
		case 'ArrowDown':
			if (snake.direction !== 'top') {
				snake.direction = 'bottom'
				break;
			}
		case 'ArrowUp':
			if (snake.direction !== 'bottom') {
				snake.direction = 'top'
				break;
			}
		case 'ArrowRight':
			if (snake.direction !== 'left') {
				snake.direction = 'right'
				break;
			}
		case 'ArrowLeft':
			if (snake.direction !== 'right') {
				snake.direction = 'left'
				break;
			}
		case 'Enter':
			clearInterval(timerId)
			break;

			
	}

}

$: if (snake.coords[0][0] < 0) snake.coords[0][0] = fieldLength - 1
$: if (snake.coords[0][0] > fieldLength - 1) snake.coords[0][0] = 0
$: if (snake.coords[0][1] < 0) snake.coords[0][1] = fieldLength - 1
$: if (snake.coords[0][1] > fieldLength - 1) snake.coords[0][1] = 0



let timerId


onMount(() => {
	timerId = setInterval(() => {
		if (snake.snakeSize < 3) snake.sizeUp()
		console.log(snake.snakeSize, snake.coords)
		switch (snake.direction) {
		case 'bottom':
			snake.coords[0][0]++
			break;
		case 'top':
			snake.coords[0][0]--
			break;
		case 'right':
			snake.coords[0][1]++
			break;
		case 'left':
			snake.coords[0][1]--
			break;
	}
	}, speed);
})





</script>

<svelte:window on:keydown={handleKeydown}/>


{#if field.length} 
<main>
	<div class="field" style="
	height: {width * fieldLength}px ;
	width: {width * fieldLength}px;
	">
		{#each field as yLine, yIndex}
			{#each yLine as xLine, xIndex}
			   <div
			   on:click={() => console.log(yIndex, xIndex)} 
			   class:snake={snake.coords[0][0] === yIndex && snake.coords[0][1] === xIndex}
			   style="width:{width}px; height:{width}px"  
			   class="dot "></div>
			{/each}
		{/each}
	</div>
</main>

{/if}

<style>

:global(*) {
	box-sizing: border-box;
	overflow: hidden;
}
main {
	padding: 50px;
}
.field {
	margin: 0 auto;
	display: flex;
	flex-wrap: wrap;
}

.dot {
	width: 15px;
	height: 15px;
	border: solid 2px rgb(96, 122, 2);
}

.snake {
	background: blueviolet
}
</style>