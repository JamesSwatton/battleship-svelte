<script>
    import { tick } from "svelte";

    export let ref;
    export let hasOverlap;
    export let numOfShipsPlaced;

    const messages = {
        start: {id: 'start', msg: "Welcome to <strong>Battleship!</strong> To get started select a ship and place it on the grid..."},
        placement1: {id: 'placement1', msg: "Great. You can also change the orientation of a ship by clicking on the arrow to the right or by pressing Space. Now go ahead and place the rest of your ships."},
        overlap: {id: 'overlap', msg: "You won't be able to place a ship if it is overlaping with another!"},
        overlap2: {id: 'overlap2', msg: "Try placing the ship on a grid square not already occupied by a ship."},
        placement2: {id: 'placement2', msg: "If you like, you can quickly place all you ships randomly by clicking on the dice icon."},
        clear: {id: 'clear', msg: "You can quickly clear the grid by clicking on the dashed square next to the dice icon."}
    }
    let msgHistory = [messages.start];
    let newMessageEl;

    $: {
        if (hasOverlap && msgHistory.includes(messages.overlap))
            addMsg(messages.overlap2)
        else if (hasOverlap) addMsg(messages.overlap)

        if (numOfShipsPlaced == 1 && !msgHistory.includes(messages.placement1)) addMsg(messages.placement1)
        else if (numOfShipsPlaced == 3 &&
            !msgHistory.includes(messages.placement2)) addMsg(messages.placement2)
        /* else if (numOfShipsPlaced == 3) addMsg(messages.placement2) */
        /* else if (numOfShipsPlaced == 4) addMsg(messages.clear) */
        console.log(msgHistory)
    }

    async function addMsg( newMsg ) {
        msgHistory = [... msgHistory, newMsg];
        await tick();
        newMessageEl.scrollIntoView();
    }
</script>

<style>
    .message-container {
        max-height: 100%;
        border-top: 2px solid #333;
        border-bottom: 2px solid #333;
        overflow: scroll;
    }

    p {
        padding-left: 30px;
    }

    .identifier {
        float: left;
    }

</style>

<div {ref} class="message-container">
    {#each msgHistory as msg, index}
        <div id={index} class="message">
            <div class="identifier" >--> </div>
            <p id={index} bind:this={newMessageEl}>{@html msg.msg}</p>
        </div>
    {/each}
</div>
