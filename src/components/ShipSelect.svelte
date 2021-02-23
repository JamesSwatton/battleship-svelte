<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();

    export let ships;

    let selected = null;

    function handleSelect(event) {
        selected = event;
        dispatch("selected", selected);
    }
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
                on:click={() => handleSelect(ship)}
                class:selected={selected === ship}
                value={ship.type}>{ship.type}</button>
        </li>
    {/each}
</ul>
