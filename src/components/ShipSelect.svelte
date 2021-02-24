<script>
    export let ships;
    export let selected;
</script>

<style>
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

<ul>
    {#each ships as ship}
        <li>
            <button
                on:click={() => (selected ? (selected = null) : (selected = ship))}
                class:selected={selected && selected.type === ship.type}
                class:placed={ship.pos.length > 0}
                value={ship.type}>{ship.type}</button>
        </li>
    {/each}
</ul>
