<script>
    export let ships;
    export let selected;

    function handleClick(ship) {
        selected = ship;
        ships[ships.findIndex(s => s.type === ship.type)].pos = [];
    }
</script>

<style>
    ul {
        list-style-type: none;
    }

    button {
        cursor: pointer;
    }

    .selected {
        border-bottom: 2px solid black;
    }

    .placed {
        background-color: lightblue;
    }
</style>

<h3>Select Ship</h3>
<hr>
<ul>
    {#each ships as ship}
        <li>
            <button
                on:click={() => handleClick(ship)}
                class:selected={selected && selected.type === ship.type}
                class:placed={ship.pos.length > 0}
                value={ship.type}>{ship.type}</button>
        </li>
    {/each}
</ul>
