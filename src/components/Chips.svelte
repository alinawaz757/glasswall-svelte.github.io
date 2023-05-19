<script>
	import { createEventDispatcher } from "svelte";

    
    const dispatch = createEventDispatcher()

	export let chips = [];
    export let label = "";
	let inputText = '';

	
    $: {
        dispatch("change", chips)
    }

	function addChip(event) {
		if (inputText.trim() !== '' && event.key === "Enter" && !chips.includes(inputText)) {
			chips = [...chips, inputText.trim()];
			inputText = '';
		}
	}

	function removeChip(index) {
		chips.splice(index, 1);
        chips = chips
	}
</script>

<h3>{label}</h3>
<div class="flex flex-wrap gap-2 p-4 ">
	{#each chips as chip, index (index)}
		<div class="bg-gray-200 rounded-full px-3 py-1 text-sm flex items-center">
			<span>{chip}</span>
			<button class="ml-2" on:click={() => removeChip(index)}>
				<svg
					class="w-4 h-4 text-gray-500 hover:text-gray-700 cursor-pointer"
					viewBox="0 0 20 20"
					fill="currentColor"
				>
					<path
						fill-rule="evenodd"
						d="M14.707 5.293a1 1 0 010 1.414L11.414 10l3.293 3.293a1 1 0 01-1.414 1.414L10 11.414l-3.293 3.293a1 1 0 01-1.414-1.414L8.586 10 5.293 6.707a1 1 0 011.414-1.414L10 8.586l3.293-3.293a1 1 0 011.414 0z"
						clip-rule="evenodd"
					/>
				</svg>
			</button>
		</div>
	{/each}

	<input
		class="w-full px-4 py-2 "
		type="text"
		placeholder="Enter text and press Enter"
		bind:value={inputText}
		on:keydown={addChip}
	/>
</div>
