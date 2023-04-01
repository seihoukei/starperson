<script>
    import Trigger from "../../utility/trigger.js"
    import ActivityButton from "../ActivityButton.svelte"
    import UIProgressBar from "./UIProgressBar.svelte"

    const ACTIVITY_NAME = "write-posts"

    export let game

    $: activity = game?.state?.activities?.writePosts
    $: stage = activity?.completed ? `Improving post` : `Writing post`
    $: quality = activity?.completed ? ["Annoying", "Boring", "Interesting", "Cool", "Awesome"][activity?.stage] : `Unknown`
</script>

<div class="activity">
    <ActivityButton activity={ACTIVITY_NAME} {game}> Write posts </ActivityButton>
    {#if activity && game.state.activity === ACTIVITY_NAME}
        <UIProgressBar max={activity?.time} current={activity?.progress} caption={stage} />
        <div class="stats">
            <div>Current quality: {quality}</div>
        </div>
        <button disabled={!activity.completed} on:click={() => Trigger("activity-post-publish")} >
            Publish current post as is
        </button>
    {/if}
</div>

<style>
</style>
