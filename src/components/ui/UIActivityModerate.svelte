<script>
    import Trigger from "../../utility/trigger.js"
    import ActivityButton from "../ActivityButton.svelte"
    import UIProgressBar from "./UIProgressBar.svelte"

    const ACTIVITY_NAME = "moderate-subreddit"

    export let game

    $: activity = game?.state?.activities?.moderateSubreddit
    $: stage = `Moderating`

</script>

<div class="activity">
    <ActivityButton activity={ACTIVITY_NAME} {game}> Moderate subreddit </ActivityButton>
    {#if activity && game.state.activity === ACTIVITY_NAME}
        <div class="hint">Moderating is depressing</div>
        <UIProgressBar max={activity?.time} current={activity?.progress} caption={stage} />
    {/if}
</div>

<style>
</style>
