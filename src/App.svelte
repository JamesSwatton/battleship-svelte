<script>
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

    let selected = null;
    let orientation = "horizontal";

    $: {
        if (selected) console.log(selected.type);
    }
</script>

<Grid bind:selected {orientation} bind:ships {state} />
<OrientationBtn on:orientation={(e) => (orientation = e.detail)} />
<ShipSelect {ships} bind:selected on:selected={(e) => (selected = e.detail)} />
