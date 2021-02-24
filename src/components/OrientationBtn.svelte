<script>
    import { createEventDispatcher } from "svelte";
    let dispatch = createEventDispatcher();

    let orientation = "horizontal";

    function handleChangeOrientation() {
        orientation = orientation === "horizontal" ? "vertical" : "horizontal";
        dispatch("orientation", orientation);
    }

    $: console.log(orientation);
</script>

<button on:click={() => handleChangeOrientation()}>Change Orientation</button>
<p>{orientation}</p>
