<script>
    import Trigger from "../../utility/trigger.js"
    import ActivityButton from "../ActivityButton.svelte"

    export let game

    $: activity = game?.state?.activities?.playGame
    $: stage = activity?.stage ? `Playing layer ${activity.stage}` : "Learning the game"
    $: progress = `${activity?.progress?.toFixed(2)}/${activity?.time?.toFixed(2)}`
</script>

    <div class="game">
        <ActivityButton activity="play-game" {game}> Play games </ActivityButton>
        {#if activity}
            <div class="stats">
                <div>Playing: {activity.title}</div>
                <div>Stage: {stage}</div>
                <div>Progress: {progress}</div>
            </div>
            <button on:click={() => Trigger("activity-game-drop")} >
                Drop current game and start new one
            </button>
        {/if}
    </div>

<style>
    div.game {
        display: flex;
        flex-direction: column;
        row-gap: 1em;
        padding: 1em 0;
        align-items: start;
    }
</style>
