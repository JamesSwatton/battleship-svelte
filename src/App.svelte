<script>
    import Grid from "./components/Grid.svelte";
    import ShipSelect from "./components/ShipSelect.svelte";
    import PlacementOption from "./components/PlacementOptions.svelte";
    import OrientationBtn from "./components/OrientationBtn.svelte";
    import Messages from "./components/Messages.svelte";
    import StartNew from "./components/StartNew.svelte";

    let states = ["placement", "game"];
    let state = states[0];
    let newGame = false;

    let activePlayer = 'player';


    // prettier-ignore
    let playerShips = [
        { type: "carrier",    size: 5, hits: [], pos: [] },
        { type: "battleship", size: 4, hits: [], pos: [] },
        { type: "cruiser",    size: 3, hits: [], pos: [] },
        { type: "submarine",  size: 3, hits: [], pos: [] },
        { type: "destroyer",  size: 2, hits: [], pos: [] },
    ];

    let opponentShips = [
        { type: "carrier",    size: 5, hits: [], pos: [] },
        { type: "battleship", size: 4, hits: [], pos: [] },
        { type: "cruiser",    size: 3, hits: [], pos: [] },
        { type: "submarine",  size: 3, hits: [], pos: [] },
        { type: "destroyer",  size: 2, hits: [], pos: [] },
    ];

    let opponentGuesses = [];

    $: numOfShipsPlaced = playerShips.filter(s => s.pos.length > 1).length;

    let selectedShip = null;
    let orientation = "horizontal";
    let hasOverlap = false;
    let playerGridEl;
    let opponentGridEl;
    let messagesEl;

    function clearShips() {
        playerShips = playerShips.map(s => {
            return {...s, pos: []}
        })
    }

    $: canStartGame = numOfShipsPlaced == 5 && state == "placement" ? true : false;

    function handleStart() {
        if (canStartGame) {
            state = states[1]
            opponentGridEl.placeRandom();
            console.log(opponentShips);
        } else {
            console.log("we can't start yet")
        }
        messagesEl.startGameMsg(canStartGame);
    }

    $: winner = () => {
        if (playerShips.map(s => s.hits).flat().length == 17) {
            return 'opponent'
        } else if (opponentShips.map(s => s.hits).flat().length == 17) {
            return 'player'
        }
    }

    function opponentTurn() {
        const getRandPos = () => {
            let randX = Math.floor(Math.random() * 10);
            let randY = Math.floor(Math.random() * 10);
            return `${randX}${randY}`;
        }

        let randPos = getRandPos();

        let hit = false;

        playerShips.forEach((s, i) => {
            if (s.pos.includes(randPos)) {
                playerShips[i] = {...s, hits:[...s.hits, randPos]};
                hit = true;
            }
        })
        if (!hit) opponentGuesses = [...opponentGuesses, randPos];
        activePlayer = "player";
    }

    $: if (activePlayer == 'opponent') setTimeout(() => opponentTurn(), 1000)
</script>

<style>
    #game-container {
        width: 720px;
        height: 640px;
        margin: auto;
        display: grid;
        grid-gap: 30px;
        grid-template-columns: repeat(9, 1fr);
        grid-template-rows: repeat(8, 1fr);
        grid-template-areas:
            "a a a a a a b b b"
            "a a a a a a b b b"
            "a a a a a a b b b"
            "a a a a a a b b b"
            "a a a a a a e e e"
            "a a a a a a d d d"
            "c c c c c c d d d"
            "c c c c c c d d d"
    }

    :global([ref=grid-1]) {
        grid-area: a;
    }

    :global([ref=grid-2]) {
        grid-area: d;
    }

    #ship-placement {
        grid-area: b;
    }

    :global([ref=messages]) {
        grid-area: c;
    }

    :global([ref=startNew]) {
        grid-area: e;
    }
</style>

<div id="game-container">
    <Grid
        ref={state == "placement" ? 'grid-1' : 'grid-2'}
        bind:this={playerGridEl}
        bind:selectedShip {orientation}
        bind:hasOverlap bind:ships={playerShips}
        guesses={opponentGuesses}
        {state}
    />

    <div id="ship-placement">
        <ShipSelect bind:ships={playerShips} bind:selectedShip />
        <hr>
        <PlacementOption on:clear={() => clearShips()} on:random={() =>
            playerGridEl.placeRandom()} {state}></PlacementOption>
        <hr>
        <OrientationBtn bind:orientation />
    </div>

    <Grid ref={state == "placement" ? 'grid-2' : 'grid-1'}
        bind:this={opponentGridEl} bind:ships={opponentShips} {state}
        hideShips={true} on:activePlayer={(e) => activePlayer = e.detail}
        {activePlayer}
    />

    <Messages ref="messages" bind:this={messagesEl} {hasOverlap} {numOfShipsPlaced} {state}/>

    <StartNew ref="startNew" on:start={() => handleStart()}
        {canStartGame} {state} {newGame}></StartNew>
</div>
