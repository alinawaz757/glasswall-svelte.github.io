<script>
	import { onMount } from 'svelte';
	import '../app.css';
	import Button from '../components/button/Button.svelte';
	import { requestTYpesData } from '../data/data';
	import { routes } from '../utils/appConfig';
	import Chips from '../components/Chips.svelte';
	import { REPLY_QUALITY_ANALYSIS } from '../utils/constants';
	import ReplyQualityForm from '../components/form/ReplyQualityForm.svelte';

	let inputMode = 'customInput';
	let selectedValue = {};
	let apiResponse = '';
	let isLoading = false;
	let tags = [];
	let replyQualityFormData = [];
	let requestTypes = requestTYpesData;

	$: {
	}

	onMount(async () => {
		const { ChipsInput, initTE } = await import('tw-elements');
		initTE({ ChipsInput });
		selectedValue = requestTypes[0];
	});

	// const handleInputModeClick = (value) => {
	// 	inputMode = value;
	// 	if (value === 'default') {
	// 		requestTypes = requestTYpesData;
	// 		selectedValue = requestTypes[0];
	// 	}
	// 	if (value === 'customInput') {
	// 		requestTypes = requestTYpesData.filter((type) => type.key !== 'all');
	// 		selectedValue = requestTypes[0];
	// 	}
	// };

	const hanldeSubmitClick = async () => {
		isLoading = true;
		try {
			const response = await fetch(routes[selectedValue.key].url, {
				method: routes[selectedValue.key].method
			});
			const data = await response.json();
			apiResponse = data;
		} catch (e) {
			console.log('error', e);
		} finally {
			isLoading = false;
		}
	};

	const handleReplyQualityFormSubmit = (formData) => {
		replyQualityFormData = [...replyQualityFormData, formData];
	};
</script>

<div class="flex flex-col md:flex-row">
	<div class="md:w-[25%] lg:w-[15%]">
		<div class="w-full md:h-[100vh] p-4 flex md:flex-col gap-4 shadow-xl">
			<!-- <Button
				variant="text"
				active={inputMode === 'default'}
				on:click={() => handleInputModeClick('default')}>Default</Button
			> -->
			<!-- on:click={() => handleInputModeClick('customInput')} -->
			<Button variant="text" active={inputMode === 'customInput'}>Custom Input</Button>
		</div>
	</div>
	<div class="md:w-[75%] lg:w-[85%] flex flex-col p-4 gap-4">
		<div class="flex flex-col md:flex-row gap-4">
			<select
				bind:value={selectedValue}
				disabled={inputMode === 'default'}
				class="md:max-w-[300px] bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block p-2.5 focus:border-2 focus:outline-none"
			>
				{#each requestTypes as requestType, i (i)}
					<option value={requestType} class="uppercase">{requestType.title}</option>
				{/each}
			</select>
			<Button on:click={hanldeSubmitClick} active={false}>Submit</Button>
		</div>
		<div class="flex flex-col gap-4 h-[100%]">
			<!-- Request -->
			<h2>Request</h2>
			<div class="h-[200px] bg-gray-100 flex px-4 py-1 gap-6">
				<div class="w-[100%] overflow-auto">
					{#if selectedValue.key !== REPLY_QUALITY_ANALYSIS}
						<Chips on:change={(event) => (tags = event.detail)} {tags} label="Text" />
					{:else}
						<ReplyQualityForm on:submit={(event) => handleReplyQualityFormSubmit(event.detail)} />
					{/if}
				</div>
				<div class="w-[100%] overflow-auto">
					{#if tags.length}
						<pre class="bg-gray-300">
                            {JSON.stringify(tags, null, 2)}
                        </pre>
					{:else if replyQualityFormData.length}
						<pre class="bg-gray-300">
                        {JSON.stringify(replyQualityFormData, null, 2)}
                    </pre>
					{/if}
				</div>
			</div>

			<!-- Response -->
			<h2>Response</h2>
			<div class="flex-grow bg-gray-100 flex items-center justify-center">
				{#if isLoading}
					<div role="status" class="min-h-[250px] md:h-[460px] flex items-center justify-center">
						<svg
							aria-hidden="true"
							class="w-14 h-14 mr-2 text-gray-200 animate-spin dark:text-gray-600 fill-blue-600"
							viewBox="0 0 100 101"
							fill="none"
							xmlns="http://www.w3.org/2000/svg"
							><path
								d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z"
								fill="currentColor"
							/><path
								d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z"
								fill="currentFill"
							/></svg
						>
						<span class="sr-only">Loading...</span>
					</div>
				{/if}
				{#if apiResponse && !isLoading}
					<div class="w-[100%] min-h-[250px] md:h-[450px] overflow-auto">
						<pre>
                        {JSON.stringify(apiResponse, null, 2)}
                    </pre>
					</div>
				{/if}
			</div>
		</div>
	</div>
</div>
