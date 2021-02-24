<script>
    export let ships;
    export let selected;
</script>

<style>
    .selected {
        border-bottom: 2px solid black;
    }
</style>

<ul>
    {#each ships as ship}
        <li>
            <button
                on:click={() => (selected = ship)}
                class:selected={selected && selected.type === ship.type}
                value={ship.type}>{ship.type}</button>
        </li>
    {/each}
</ul>
