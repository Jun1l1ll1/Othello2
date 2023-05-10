<script>
    export let isBlacksTurn = true;
    export let tilesOnBoard = {};

    export let color = 'rose';

    let scoreing = [0, 0]
    function setScoring() {
        scoreing = [0, 0]
        for (let i of Object.keys(tilesOnBoard)) {
            tilesOnBoard[i] == true ? scoreing = [scoreing[0]+1, scoreing[1]] : scoreing = [scoreing[0], scoreing[1]+1]
        }
    }
    $:tilesOnBoard, setScoring();

    function skip() {
        if (confirm("Skip your turn?")) {
            isBlacksTurn = !isBlacksTurn;
        }
    }
</script>

<div class="w-1/5 absolute top-1/4 left-[77%] text-zinc-{isBlacksTurn?'900':'200'}">
    <h2 class="text-2xl font-bold">
        {#if isBlacksTurn}
        Black's Turn
        {:else}
        Whites's Turn
        {/if}
    </h2>

    <br/>
    <div class="w-full h-fit flex">
        <div class="w-[{Math.round((scoreing[0]/(scoreing[0]+scoreing[1]))*100)}%] bg-zinc-900 text-zinc-200 p-1 rounded">{Math.round((scoreing[0]/(scoreing[0]+scoreing[1]))*100)}%</div>
        <div class="w-[{100 - Math.round((scoreing[0]/(scoreing[0]+scoreing[1]))*100)}%] bg-zinc-200 text-zinc-900 p-1 rounded text-right">{Math.round((scoreing[1]/(scoreing[0]+scoreing[1]))*100)}%</div>
    </div>

    <br/>
    <p>No place for your piece?</p>
    <button on:click={skip} class="bg-{color}-500 hover:bg-{color}-400 p-1 rounded text-{color}-900">Skip Turn</button>
</div>