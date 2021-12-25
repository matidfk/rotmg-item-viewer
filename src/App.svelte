<script lang="ts">
import Item from "./Item.svelte";
	let size = 8;
	async function getObjects() {
		let res = await fetch(
			"https://static.drips.pw/rotmg/production/current/json/Objects.json"
		);
		let json = await res.json();
		return json.Object;
	}
	const obj = getObjects();
	let tooltipItem;
</script>

<main>
	<div id="filters">
		idk filters go here when i add them
	</div>
	<div id="items" style="grid-template-columns: repeat(auto-fill, {14 * size}px;">
		{#await obj}
			<p>Loading...</p>
		{:then obj}
			{#each obj as o}
				<Item item={o} size={size} on:enter={(e) => tooltipItem = JSON.stringify(e.detail.item, null, 10)} on:leave={() => tooltipItem = null}/>
			{/each}
		{:catch error}
			<p style="color: red">{error.message}</p>
		{/await}
	</div>
	<div id="tooltip" bind:textContent={tooltipItem} contenteditable="true" style="opacity: {tooltipItem ? 100 : 0}%; background-color: rgba(0, 0, 0, {tooltipItem ? 0.7 : 0}; visibility: {tooltipItem ? "visible" : "hidden"})">
	</div>
</main>

<style>
	#items {
		display: grid;
		
		justify-content: space-between;
		width: calc(100vw - var(--item-margin));
		overflow: hidden;
	}

	#filters {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		z-index: 99;
		box-shadow:  0px 5px 3px black;
		height: 5rem;
		background-color: red;
	}

	#tooltip {
		pointer-events:none;
        border-radius: 0.5rem;
        text-shadow: 2px 2px 3px black;
        position: fixed;
        top: 6rem;
        left: 50vw;
        transform: translateX(-50%);
        transition-duration: 0.2s;
        background-color: rgba(0, 0, 0, 0);
        color: white;
        white-space: pre-wrap;
    }
</style>
