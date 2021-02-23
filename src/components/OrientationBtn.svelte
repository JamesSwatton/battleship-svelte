<script>
    let orientation = "horizontal";

    function changeOrientation() {
        orientation = orientation === "horizontal" ? "vertical" : "horizontal";
    }

    $: console.log(orientation);
</script>

<button on:click={() => changeOrientation()}>Change Orientation</button>
<p>{orientation}</p>
