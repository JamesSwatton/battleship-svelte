<script>
    import ShipSelect from "./components/ShipSelect.svelte";
    import Grid from "./components/Grid.svelte";
    import OrientationBtn from "./components/OrientationBtn.svelte";

    // prettier-ignore
    const ships = [
        { type: "carrier",    size: 5, hits: [], pos: [] },
        { type: "battleship", size: 4, hits: [], pos: [] },
        { type: "cruiser",    size: 3, hits: [], pos: [] },
        { type: "submarine",  size: 3, hits: [], pos: [] },
        { type: "destroyer",  size: 2, hits: [], pos: [] },
    ];

    let selected = null;

    $: {
        if (selected) console.log(selected.type);
    }
</script>

<Grid {selected} />
<OrientationBtn />
<ShipSelect {ships} on:selected={(e) => (selected = e.detail)} />
