<script>
    import Trigger from "../utility/trigger.js"

    export let game
    export let activity
</script>

<button on:click={() => Trigger("set-activity", activity)} >
    <slot></slot> {#if game?.state?.activity === activity}(active){/if}
</button>
