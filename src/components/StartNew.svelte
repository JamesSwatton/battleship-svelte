<script>
    import { createEventDispatcher } from 'svelte';
    import { fade } from 'svelte/transition';

    const dispatch = createEventDispatcher();

    export let ref;
    export let state;
    export let canStartGame;
    export let newGame;
</script>

<style>
    .alert-container {
        text-align: center;
        cursor: pointer;
    }

    .inactive {
        background-color: none;
        color: #333;
        border: 2px dotted #333;
        transition: color 1s, background-color 1s;
    }

    .active {
        border: 2px solid #333;
        box-shadow: 10px 10px 30px #333;
    }
</style>

{#if state == "placement"}
    {#if canStartGame}
        <div transition:fade {ref} class="alert-container active" on:click={() =>
            dispatch("start")}>
            <h3>Start Game</h3>
        </div>
    {:else}
        <div transition:fade {ref} class="alert-container inactive" on:click={() =>
            dispatch("start")}>
            <h3>Start Game</h3>
        </div>
    {/if}
{:else}
    {#if newGame}
        <div transition:fade {ref} class="alert-container active" on:click={() =>
            dispatch("new")}>
            <h3>Start Game</h3>
        </div>
    {:else}
        <div transition:fade {ref} class="alert-container inactive" on:click={() =>
            dispatch("new")}>
            <h3>New Game</h3>
        </div>
    {/if}
{/if}
