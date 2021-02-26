<script>
    export let orientation;
</script>

<button on:click={() => orientation = orientation === "horizontal" ? "vertical" : "horizontal"}>Change Orientation</button>
<p>{orientation}</p>
