
<div class="flex items-center justify-center h-screen w-screen">

    <div class="grid grid-cols-8 w-[50%] overflow-hidden rounded border-[1.5rem] border-zinc-{isBlacksTurn?'900':'200'}">
        {#each {length: (8*8)} as _, i}
        <button on:click={() => tileClicked(i)} class="{(Math.floor(i/8)+i%8)%2 == 0 ? 'bg-'+color+'-500':'bg-'+color+'-400'} aspect-square flex items-center justify-center">
            {#if tilesOnBoard.hasOwnProperty(i)}
            <Checker isBlack={tilesOnBoard[i]}/>
            {/if}
        </button>
        {/each}
    </div>

    <TextBox color={color} bind:isBlacksTurn={isBlacksTurn} bind:tilesOnBoard={tilesOnBoard}/>
    <Victory color={color} bind:hasWon={hasWon} bind:blackWon={isBlacksTurn} bind:tilesOnBoard={tilesOnBoard}/>
    <Color bind:color={color} isBlacksTurn={isBlacksTurn}/>

</div>


<script>
    import Checker from "../lib/component/Checker.svelte";
    import TextBox from "../lib/component/TextBox.svelte";
    import Victory from "../lib/component/Victory.svelte";
    import Color from "../lib/component/Color.svelte";

    let color = 'rose';

    let hasWon = false;

    let tilesOnBoard = {27:false, 28:true, 35:true, 36:false}
    let isBlacksTurn = true;

    function tileClicked(pos) {
        if (!tilesOnBoard.hasOwnProperty(pos) && canSwapStuff(pos).some((e) => e)) {
            placeChecker(pos);
        }
    }

    function placeChecker(pos) {
        console.log('x:',pos%8,'y:',Math.floor(pos/8))
        tilesOnBoard[pos] = isBlacksTurn;
        swapCheckers(pos, canSwapStuff(pos));
        isBlacksTurn = !isBlacksTurn;
    }

    function swapCheckers(placedPos, canSwapStuffArray) {
        console.log(canSwapStuffArray);

        function swap(xdir, ydir) { // xdir = -1:left, 0:stay, 1:right    ydir = -1:up, 0:stay, 1:down
            let pos = placedPos+xdir+(ydir*8);
            while (tilesOnBoard.hasOwnProperty(pos) && tilesOnBoard[pos] != tilesOnBoard[placedPos]) {
                tilesOnBoard[pos] = !tilesOnBoard[pos];
                pos += xdir+(ydir*8);
            }
        }

        // left
        if (canSwapStuffArray[1]) {
            swap(-1,0);
        }

        // right
        if (canSwapStuffArray[6]) {
            swap(1, 0);
        }

        // up
        if (canSwapStuffArray[3]) {
            swap(0, -1);
        }

        // down
        if (canSwapStuffArray[4]) {
            swap(0, 1);
        }

        // left-up
        if (canSwapStuffArray[0]) {
            swap(-1, -1);
        }

        // left-down
        if (canSwapStuffArray[2]) {
            swap(-1, 1);
        }

        // right-up
        if (canSwapStuffArray[5]) {
            swap(1, -1);
        }

        // right-down
        if (canSwapStuffArray[7]) {
            swap(1, 1);
        }

    }



    function canSwapStuff(placedPos) {
        let canSwapInDir = []
        for (let x = -1; x < 2; x++) { // -1:left, 0:stay, 1:right
            for (let y = -1; y < 2; y++) { // -1:up, 0:stay, 1:down
                let pos = placedPos;
                let canSwapThis = false;
                if (x == 0 && y == 0) {
                    continue;
                }

                let count = 0
                while ((pos%8 >= -x && pos%8 < 8-x) && (Math.floor(pos/8) >= -y && Math.floor(pos/8) < 8-y) && tilesOnBoard.hasOwnProperty(pos+x+(y*8))) {
                    pos += x+(y*8);
                    if (count != 0 && tilesOnBoard[pos] == isBlacksTurn) {
                        canSwapThis = true;
                        break;
                    } else if (count == 0 && tilesOnBoard[pos] == isBlacksTurn) {
                        break;
                    }
                    count += 1;
                }
                canSwapInDir.push(canSwapThis);
            }
        }
        return canSwapInDir;


        // Arrayen som blir returnert og tilsvarende skjekk sin posisjon
        // 0 3 5
        // 1   6
        // 2 4 7
    }


</script>