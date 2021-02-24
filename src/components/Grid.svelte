<script>
    import { onMount } from "svelte";

    export let state;
    export let ships;
    export let selected;
    export let orientation;

    let currentPos = null;

    let allPos = [];

    $: {
        if (selected) {
            allPos = ships
                .map((s) => (s.type !== selected.type ? s.pos : []))
                .flat();
        } else {
            allPos = ships.map((s) => s.pos).flat();
        }
    }

    let ids = [];

    function createIDs() {
        for (let y = 0; y < 10; y++) {
            for (let x = 0; x < 10; x++) {
                ids = [...ids, `${x}${y}`];
            }
        }
    }

    function handleMouseEnter(id) {
        currentPos = id;
        updateShipPos();
    }

    function handleClick(id) {
        if (state === "placement" && selected) {
            saveShipPos();
        } else if (state === "placement" && !selected) {
            ships.forEach((s) => {
                if (s.pos.includes(id)) selected = s;
            });
        }
    }

    function updateShipPos() {
        if (selected && currentPos) {
            let parsedCurrentPos = currentPos.split("").map((c) => parseInt(c));
            let x = parsedCurrentPos[0];
            let y = parsedCurrentPos[1];
            let pos = [];
            const constrain = (pos, size) =>
                pos > 10 - size ? 10 - size : pos;
            if (orientation === "horizontal") {
                x = constrain(parsedCurrentPos[0], selected.size);
                for (let i = x; i < x + selected.size; i++) {
                    pos.push(`${i}${y}`);
                }
            } else {
                y = constrain(parsedCurrentPos[1], selected.size);
                for (let j = y; j < y + selected.size; j++) {
                    pos.push(`${x}${j}`);
                }
            }
            selected = { ...selected, pos: pos };
        }
    }

    function saveShipPos() {
        const hasNoOverlap = () =>
            selected.pos.every((e) => !allPos.includes(e));
        if (hasNoOverlap()) {
            let index = ships.findIndex((e) => e.type === selected.type);
            ships[index] = selected;
            selected = null;
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

    .ship {
        background-color: lightblue;
    }

    .selectedShip {
        background-color: cyan;
    }

    .overlap {
        background-color: red;
    }
</style>

<div
    id="grid-container"
    on:mouseleave={() => {
        if (selected) selected.pos = [];
    }}>
    {#each ids as id}
        <div
            {id}
            class="grid-square"
            on:mouseenter={() => handleMouseEnter(id)}
            on:click={() => handleClick(id)}
            class:ship={allPos.includes(id)}
            class:selectedShip={selected && selected.pos.includes(id)}
            class:overlap={allPos.includes(id) && selected && selected.pos.includes(id)}>
            {id}
        </div>
    {/each}
</div>
