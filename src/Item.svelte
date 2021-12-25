<script lang="ts">
    import { object_without_properties } from "svelte/internal";
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    export let item: object;
    export let size: number = 8;

    function getXoffset(hex) {
        let dec = parseInt(hex, 16);
        return -(dec % 16) * 8 * size;
    }

    function getYoffset(hex) {
        let dec = parseInt(hex, 16);
        return -Math.floor(dec / 16) * 8 * size;
    }

    function getImageUrl(sheet) {
        return (
            "https://static.drips.pw/rotmg/production/current/sheets/" +
            sheet +
            ".png"
        );
    }
</script>

{#if item.hasOwnProperty("Texture")}
    <div
        class="outer"
        style="width: {size * 8}px;
    height: {size * 8}px;
    padding: {size}px;
    margin: {2 * size}px;
    border-radius: {size}px"
    on:mouseenter={() => dispatch('enter', {
        item: item
    })}
    on:mouseleave={() => dispatch('leave')}
    >
        <div
            class="inner"
            style="width: {size * 8}px;
                height: {size * 8}px;
                background-position-x: {getXoffset(item.Texture.Index)}px;
                background-position-y: {getYoffset(item.Texture.Index)}px;
                background-image: url('{getImageUrl(item.Texture.File)}')"
        >
    </div>
    </div>
{/if}

<style>
    .inner {
        image-rendering: pixelated;
        background-size: 1600%;
        filter: drop-shadow(2px 0px 0px black) drop-shadow(-2px -0px 0px black)
            drop-shadow(0px 2px 0px black) drop-shadow(0px -2px 0px black)
            drop-shadow(0px 0px 4px black);
    }

    .outer {
        background-color: rgba(0, 0, 0, 0.2);
    }
</style>
