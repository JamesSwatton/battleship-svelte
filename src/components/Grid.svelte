<script>
    import { onMount } from "svelte";

    export let ref;
    export let state;
    export let ships;
    export let selectedShip;
    export let orientation;
    export let hasOverlap;

    let currentPos;

    let ids = [];
    onMount(() => createIDs());

    $: allPos = () => {
        if (selectedShip) {
            return ships
                .map((s) => (s.type !== selectedShip.type ? s.pos : []))
                .flat();
        } else {
            return ships.map((s) => s.pos).flat();
        }
    }

    $: if(orientation && selectedShip && currentPos) updateShipPos();

    function createIDs() {
        for (let y = 0; y < 10; y++) {
            for (let x = 0; x < 10; x++) {
                ids = [...ids, `${x}${y}`];
            }
        }
    }

    function handleMouseLeave() {
        if (selectedShip) selectedShip.pos = [];
        currentPos = null;
    }

    function handleClick() {
        if (state === "placement" && selectedShip) {
            if (saveShipPos(selectedShip)) selectedShip = null;
        } else if (state === "placement" && !selectedShip) {
            // select already placed ship
            ships.forEach((s) => {
                if (s.pos.includes(currentPos)){
                    let sel = {...s}
                    // clear ship positions to update select button state
                    ships[ships.findIndex(s => s.type === sel.type)].pos = [];
                    selectedShip = sel;
                }
            });
        }
    }

    function updateShipPos() {
        if (selectedShip) {
            let parsedCurrentPos = currentPos.split("").map((c) => parseInt(c));
            let x = parsedCurrentPos[0];
            let y = parsedCurrentPos[1];
            let pos = [];
            const constrain = (pos, size) =>
                pos > 10 - size ? 10 - size : pos;
            if (orientation === "horizontal") {
                x = constrain(parsedCurrentPos[0], selectedShip.size);
                for (let i = x; i < x + selectedShip.size; i++) {
                    pos.push(`${i}${y}`);
                }
            } else {
                y = constrain(parsedCurrentPos[1], selectedShip.size);
                for (let j = y; j < y + selectedShip.size; j++) {
                    pos.push(`${x}${j}`);
                }
            }
            selectedShip = { ...selectedShip, pos: pos };
        }
    }

    function saveShipPos() {
        const hasNoOverlap = () =>
            selectedShip.pos.every((e) => !allPos().includes(e));
        if (hasNoOverlap()) {
            hasOverlap = false;
            let index = ships.findIndex((e) => e.type === selectedShip.type);
            ships[index] = selectedShip;
            console.log('hi')
            return true;
        }
        hasOverlap = true;
        return false;
    }

    export function placeRandom() {
        const dirs = ["horizontal", "vertical"];
        const randDir = () => dirs[Math.floor(Math.random() * 2)];
        const randPos = () => {
            let randX = Math.floor(Math.random() * 10);
            let randY = Math.floor(Math.random() * 10);
            return `${randX}${randY}`;
        }

        ships.forEach(ship => {
            currentPos = randPos();
            orientation = randDir();
            selectedShip = ship;
            updateShipPos()
            while (!saveShipPos()) {
                currentPos = randPos();
                orientation = randDir();
                updateShipPos()
            }
            selectedShip = null;
            currentPos = null;
        });
    }

</script>

<style>
    .grid-container {
        display: grid;
        grid-template-columns: repeat(10, 1fr);
        grid-template-rows: repeat(10, 1fr);
        grid-gap: 2px;
        box-shadow: 10px 10px 30px #333;
    }

    .grid-square {
        background-color:deepskyblue;
    }

    .ship {
        cursor: pointer;
        background-color: darkgray;
    }

    .selectedShip {
        border: none;
        background-color: lightgray;
        position: relative;
        top: -8px;
        left: -8px;
    }

    .overlap {
        background-color: tomato;
    }
</style>

<div
    {ref}
    class="grid-container"
    on:mouseleave={() => handleMouseLeave()}>
    {#each ids as id}
        <div
            {id}
            class="grid-square"
            on:mouseenter={() => currentPos = id}
            on:click={() => handleClick(id)}
            class:ship={allPos().includes(id)}
            class:selectedShip={selectedShip && selectedShip.pos.includes(id)}
            class:overlap={allPos().includes(id) && selectedShip && selectedShip.pos.includes(id)}>
        </div>
    {/each}
</div>
