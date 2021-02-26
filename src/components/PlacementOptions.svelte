<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
</script>

<style>
    div {
        padding: 0;
        margin: 0;
        display: inline-block;
        margin-left: 20px;
        cursor: pointer;
        font-size: 30px;
    }
</style>

<div on:click={() => dispatch("random")}>&#9860;</div>
<div on:click={() => dispatch("clear")}>&#11034;</div>
