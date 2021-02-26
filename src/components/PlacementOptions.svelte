<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
</script>

<button on:click={() => dispatch("random")}>Random</button>
<button on:click={() => dispatch("clear")}>Clear</button>
