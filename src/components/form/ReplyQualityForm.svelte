<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	const handleSubmit = (event) => {
		const formEl = event.target;
		const formData = new FormData(formEl);
		const text = formData.get('text');
		const suggestion = formData.get('suggestion');
		const arch = formData.get('arch');
		dispatch('submit', { text, suggestion, arch });
        formEl.reset()
	};
</script>

<form class="flex flex-col gap-2 px-2" on:submit|preventDefault={handleSubmit}>
	<div class="flex justify-between pr-2 pb-2">
		<h3 class="mb-1">Reply Quality Form</h3>
		<button
			type="submit"
			class="inline-block bg-primary rounded px-4 text-white hover:text-white hover:bg-blue-500 font-medium uppercase leading-normal transition duration-150 ease-in-out focus:outline-none"
			>Add</button
		>
	</div>
	<input class="w-full px-4 py-2" type="text" name="text" placeholder="Text" required/>
	<input class="w-full px-4 py-2" type="text" name="suggestion" placeholder="Suggestion" required/>
	<input class="w-full px-4 py-2" type="text" name="arch" placeholder="Arch" required/>
</form>
