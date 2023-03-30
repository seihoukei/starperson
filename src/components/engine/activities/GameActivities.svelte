<script>
    import ActivityPlayGame from "./ActivityPlayGame.svelte"
    import {onDestroy, onMount} from "svelte"
    import Trigger from "../../../utility/trigger.js"
    import ActivityWritePosts from "./ActivityWritePosts.svelte"
    import ActivityModerateSubreddit from "./ActivityModerateSubreddit.svelte"

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
{:else if state.activity === "write-posts"}
    <ActivityWritePosts {state} bind:activity={activities.writePosts} />
{:else if state.activity === "moderate-subreddit"}
    <ActivityModerateSubreddit {state} bind:activity={activities.moderateSubreddit} />
{/if}
