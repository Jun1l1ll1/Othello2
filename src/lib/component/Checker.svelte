<script>
    import { scale } from 'svelte/transition';

    export let isBlack = true;
</script>

{#if isBlack}
<div in:scale class="w-[90%] h-[90%] bg-zinc-900 rounded-full"></div>
{:else}
<div in:scale class="w-[90%] h-[90%] bg-zinc-200 rounded-full"></div>
{/if}
