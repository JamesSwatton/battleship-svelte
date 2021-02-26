<script>
    export let orientation;

    function changeOrientation(event) {
        if (event && event.keyCode === 32) {
            orientation = orientation === "horizontal" ? "vertical" : "horizontal"
            return;
        }
        orientation = orientation === "horizontal" ? "vertical" : "horizontal"
    }
</script>

<svelte:window on:keydown={(e) => changeOrientation(e)} />
<button on:click={() => changeOrientation()}>Change Orientation</button>
<p>{orientation}</p>
