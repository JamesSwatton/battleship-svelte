<script>
    import { onMount } from "svelte";

    export let selected;
    export let orientation;

    let currentPos = null;

    let ids = [];

    function createIDs() {
        for (let y = 0; y < 10; y++) {
            for (let x = 0; x < 10; x++) {
                ids = [...ids, `${x}${y}`];
            }
        }
        console.log(ids);
    }

    function handleMouseEnter(id) {
        currentPos = id;
        updateShipPos();
    }

    function updateShipPos() {
        if (selected && currentPos) {
            let parsedCurrentPos = currentPos.split("").map((c) => parseInt(c));
            let x = parsedCurrentPos[0];
            let y = parsedCurrentPos[1];
            let pos = [];
            if (orientation === "horizontal") {
                for (let i = x; i < x + selected.size; i++) {
                    pos.push(`${i}${y}`);
                }
            } else {
                for (let j = y; j < y + selected.size; j++) {
                    pos.push(`${x}${j}`);
                }
            }
            selected = { ...selected, pos: pos };
        }
    }

    onMount(() => createIDs());
</script>

<style>
    #grid-container {
        width: 500px;
        height: 500px;
        display: grid;
        grid-template-columns: repeat(10, 1fr);
        grid-template-rows: repeat(10, 1fr);
    }

    .grid-square {
        border: 1px solid black;
    }

    .grid-square:hover {
        background-color: lightgray;
    }

    .ship {
        background-color: cyan;
    }
</style>

<div id="grid-container" on:mouseleave={() => (currentPos = null)}>
    {#each ids as id}
        <div
            {id}
            class="grid-square"
            on:mouseenter={() => handleMouseEnter(id)}
            class:ship={selected && selected.pos.includes(id)}>
            {id}
        </div>
    {/each}
</div>
