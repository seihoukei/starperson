<script>
    import Trigger from "../../utility/trigger.js"
    import ActivityButton from "../ActivityButton.svelte"

    export let game

    $: activity = game?.state?.activities?.writePosts
    $: stage = activity?.completed ? `Improving post` : `Writing post`
    $: progress = `${activity?.progress?.toFixed(2)}/${activity?.time?.toFixed(2)}`
    $: quality = activity?.completed ? ["Annoying", "Boring", "Interesting", "Cool", "Awesome"][activity?.stage] : `Unknown`
</script>

<div class="game">
    <ActivityButton activity="write-posts" {game}> Write posts </ActivityButton>
    {#if activity}
        <div class="stats">
            <div>Stage: {stage}</div>
            <div>Progress: {progress}</div>
            <div>Current quality: {quality}</div>
        </div>
        <button disabled={!activity.completed} on:click={() => Trigger("activity-post-publish")} >
            Publish current post as is
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
