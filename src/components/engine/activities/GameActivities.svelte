<script>
    import ActivityPlayGame from "./ActivityPlayGame.svelte"
    import {onDestroy, onMount} from "svelte"
    import Trigger from "../../../utility/trigger.js"

    export let state
    export let activity = ""
    export let activities = {}

    $: activities ??= {}

    function setActivity(newActivity) {
        activity = newActivity
    }

    const triggers = []
    onMount(() => {
        triggers.push(Trigger.on("set-activity", setActivity))
    })

    onDestroy(() => {
        triggers.forEach(trigger => trigger.cancel())
    })

</script>

{#if state.activity === "play-game"}
    <ActivityPlayGame {state} bind:activity={activities.playGame} />
{/if}
