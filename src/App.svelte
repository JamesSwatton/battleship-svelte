<script>
    import PlacementOption from "./components/PlacementOptions.svelte";
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

    let selectedShip = null;
    let orientation = "horizontal";
    let grid;

    function clearShips() {
        ships = ships.map(s => {
            return {...s, pos: []}
        })
    }
</script>

<style>
    #game-container {
        display: inline-flex;
    }

    #ship-placement {
        display: inline-block;
    }
</style>

<div id="game-container">
    <Grid bind:this={grid} bind:selectedShip {orientation} bind:ships {state} />
    <div id="ship-placement">
        <ShipSelect bind:ships bind:selectedShip />
        <hr>
        <OrientationBtn bind:orientation />
        <hr>
        <PlacementOption on:clear={() => clearShips()} on:random={() =>
            grid.placeRandom()}></PlacementOption>
    </div>
</div>
