<script>
    import { fade } from 'svelte/transition';
    import Checker from "./Checker.svelte";

    export let color = 'rose';
    export let  isBlacksTurn = true;

    let colorsToChoose = ['orange', 'rose', 'violet', 'blue', 'emerald', 'zinc'];
</script>

<div class="bg-zinc-{isBlacksTurn?'900':'200'} rounded absolute right-10 bottom-10">
    <p class="absolute right-[3.5rem] text-zinc-{isBlacksTurn?'900':'200'}" style="writing-mode: vertical-lr; text-orientation: upright;">Colors</p>
    {#each colorsToChoose as c} 
    <button on:click={() => color = c} class="bg-{c}-500 w-10 h-10 rounded m-2 flex items-center justify-center">
        {#if c == color}
        <div class="w-[85%] h-[85%] bg-zinc-{isBlacksTurn?'900':'200'} rounded-full"></div>
        {/if}
    </button>
    {/each}
</div>

{#if color == 'zinc'}
<p in:fade out:fade class="absolute bottom-10 left-10 text-zinc-700">Isn't this a bit of a boring color..?</p>
{/if}