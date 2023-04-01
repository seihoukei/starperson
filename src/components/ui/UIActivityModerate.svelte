<script>
    import Trigger from "../../utility/trigger.js"
    import ActivityButton from "../ActivityButton.svelte"

    const ACTIVITY_NAME = "moderate-subreddit"

    export let game

    $: activity = game?.state?.activities?.moderateSubreddit
    $: stage = `Moderating`
    $: progress = `${activity?.progress?.toFixed(2)}/${activity?.time?.toFixed(2)}`

</script>

<div class="game">
    <ActivityButton activity={ACTIVITY_NAME} {game}> Moderate subreddit </ActivityButton>
    {#if activity && game.state.activity === ACTIVITY_NAME}
        <div class="hint">Moderating is depressing</div>
        <div class="stats">
            <div>Stage: {stage}</div>
            <div>Progress: {progress}</div>
        </div>
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
