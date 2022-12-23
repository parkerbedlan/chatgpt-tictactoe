<script lang="ts">
	import { writable } from 'svelte/store';

	export let cells = writable(new Array(9).fill(null));
	export let player = writable('x');

	function checkWinner(cells: any) {
		const lines = [
			[0, 1, 2],
			[3, 4, 5],
			[6, 7, 8],
			[0, 3, 6],
			[1, 4, 7],
			[2, 5, 8],
			[0, 4, 8],
			[2, 4, 6]
		];

		for (const line of lines) {
			const [a, b, c] = line;
			if (cells[a] && cells[a] === cells[b] && cells[a] === cells[c]) {
				return cells[a];
			}
		}

		return null;
	}

	function handleClick(cell: any, i: number) {
		console.log(cell);
		cells.update((cells) => {
			if (cells[i] || checkWinner(cells)) {
				return cells;
			}
			cells[i] = $player;
			player.update((p) => (p === 'x' ? 'o' : 'x'));
			return cells;
		});
	}
</script>

<div class="flex justify-center">
	<div class="w-64 h-64 flex flex-col items-center">
		<div class="text-2xl font-bold mb-4">Tic Tac Toe</div>
		<div class="flex flex-col justify-between">
			<div class="grid grid-rows-3 grid-cols-3 gap-4">
				{#each $cells as cell, i}
					<div
						class="h-32 w-32 flex items-center justify-center border bg-gray-400"
						on:click={() => handleClick(cell, i)}
					>
						{#if cell === 'x'}
							<div class="text-3xl text-red-500">X</div>
						{:else if cell === 'o'}
							<div class="text-3xl text-blue-500">O</div>
						{/if}
					</div>
				{/each}
			</div>
			<div class="text-2xl font-bold mt-4">{$player}'s turn</div>
		</div>
	</div>
</div>
