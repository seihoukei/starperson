<script>
    import Trigger from "../../utility/trigger.js"
    import ActivityButton from "../ActivityButton.svelte"
    import UIProgressBar from "./UIProgressBar.svelte"

    export let game

    const ACTIVITY_NAME = "play-game"

    $: activity = game?.state?.activities?.playGame
    $: stage = activity?.stage ? `Prestige layer ${activity.stage}` : "Learning the game"
</script>

    <div class="activity">
        <ActivityButton activity={ACTIVITY_NAME} {game}> Play games </ActivityButton>
        {#if activity && game.state.activity === ACTIVITY_NAME}
            <div class="hint">Playing games is lightening</div>
            <div class="stats">
                <div>Playing: {activity.title}</div>
            </div>
            <UIProgressBar max={activity?.time} current={activity?.progress} caption={stage} />
            <button on:click={() => Trigger("activity-game-drop")} >
                Drop current game and start new one
            </button>
        {/if}
    </div>

<style>
</style>
