<script>
	import { createEventDispatcher } from "svelte";


    export let active;
    export let type= "button";
    export let variant = "standard";

    const dispatch = createEventDispatcher()
    const hanldeClick = () => {
        dispatch("click")
    }

</script>

<button
	type={type}
    on:click={hanldeClick}
	class="inline-block pb-2 pt-2.5 {active ? 'text-primary ':''} {variant === "standard" ? 'bg-primary rounded py-2 px-4 text-white':''} {variant === "standard" ? 'hover:text-white hover:bg-blue-500':'hover:text-primary'} font-medium uppercase leading-normal  transition duration-150 ease-in-out focus:outline-none"
>
	<slot />
</button>
