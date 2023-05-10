<script>
    export let hasWon = false;
    export let blackWon = true;
    export let tilesOnBoard = {};

    export let color = 'rose';


    $: tilesOnBoard, boardIsFull();
    function boardIsFull() {
        if (Object.keys(tilesOnBoard).length == 64) {
            if (64 - calcScore() < 32) {
                blackWon = true;
            } else if (64 - calcScore() > 32) {
                blackWon = false;
            } else {
                blackWon = tralse; // Fix if equal score
            }
            hasWon = true;
        }
    }

    function calcScore() {
        let blackScore = 0;
        for (let i of Object.values(tilesOnBoard)) {
            console.log(i);
            i ? blackScore++ : '';
        }
        return blackScore;
    }


    function newGame() {
        if (confirm("Start a new game?")) {
            tilesOnBoard = {27:false, 28:true, 35:true, 36:false};
            blackWon = true;
            hasWon = false;
        }
    }
</script>

{#if hasWon} 
<div class="absolute top-1/4 right-[77%] text-right text-zinc-{blackWon?'900':'200'}">
    <h2 class="text-4xl font-bold">{#if blackWon}Black{:else}White{/if} Won!</h2>
    <p>A total of {#if blackWon}{calcScore()}{:else}{64-calcScore()}{/if} {#if blackWon}Black{:else}White{/if} pieces</p> 

    <br/>
    <p>Want to play again?</p>
    <button on:click={newGame} class="bg-{color}-500 hover:bg-{color}-400 p-2 rounded text-2xl text-{color}-900">New Game</button>
</div>
{/if}
