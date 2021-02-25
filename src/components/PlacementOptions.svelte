<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
</script>

<button>Random</button>
<button on:click={() => dispatch("clear")}>Clear</button>
