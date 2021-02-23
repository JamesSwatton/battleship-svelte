<script>
    import { onMount } from "svelte";
    export let selected;

    let ids = [];

    function createIDs() {
        for (let y = 0; y < 10; y++) {
            for (let x = 0; x < 10; x++) {
                ids = [...ids, `${x}${y}`];
            }
        }
        console.log(ids);
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
</style>

<div id="grid-container">
    {#each ids as id}
        <div {id} class="grid-square">{id}</div>
    {/each}
</div>
