<script>
    import {onDestroy, onMount} from "svelte"
    import Trigger from "../../../../utility/trigger.js"

    export let state
    export let subreddit

    function createSubreddit() {
        subreddit = Object.create(null)
        subreddit.name = Math.random()
    }

    const triggers = []
    onMount(() => {
        triggers.push(Trigger.on("create-subreddit", createSubreddit))
    })

    onDestroy(() => {
        triggers.forEach(trigger => trigger.cancel())
    })

</script>

{#if subreddit}
{/if}
