<script>
    import PlacementOption from "./components/PlacementOptions.svelte";
    import Messages from "./components/Messages.svelte";
    import ShipSelect from "./components/ShipSelect.svelte";
    import Grid from "./components/Grid.svelte";
    import OrientationBtn from "./components/OrientationBtn.svelte";

    let states = ["placement", "game"];

    let state = states[0];

    // prettier-ignore
    let ships = [
        { type: "carrier",    size: 5, hits: [], pos: [] },
        { type: "battleship", size: 4, hits: [], pos: [] },
        { type: "cruiser",    size: 3, hits: [], pos: [] },
        { type: "submarine",  size: 3, hits: [], pos: [] },
        { type: "destroyer",  size: 2, hits: [], pos: [] },
    ];

    $: numOfShipsPlaced = ships.filter(s => s.pos.length > 1).length;

    let selectedShip = null;
    let orientation = "horizontal";
    let hasOverlap = false;
    let grid;

    function clearShips() {
        ships = ships.map(s => {
            return {...s, pos: []}
        })
    }

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
            "a a a a a a b b b"
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


</style>

<div id="game-container">
    <Grid ref="grid-1" bind:this={grid} bind:selectedShip {orientation}
        bind:hasOverlap bind:ships {state} />
    <div id="ship-placement">
        <ShipSelect bind:ships bind:selectedShip />
        <hr>
        <PlacementOption on:clear={() => clearShips()} on:random={() =>
            grid.placeRandom()}></PlacementOption>
        <hr>
        <OrientationBtn bind:orientation />
    </div>
    <Grid ref="grid-2" bind:this={grid} bind:selectedShip {orientation}
        bind:ships {state} />
    <Messages ref="messages" {hasOverlap} {numOfShipsPlaced} />

</div>
