<script>
    import {onDestroy, onMount} from "svelte"
    import Trigger from "../../../utility/trigger.js"

    export let state
    export let activity = {}

    if (activity.stage === undefined) {
        newModeration()
    }

    function newModeration() {
        activity.time = 1
        activity.progress = 0
    }

    function completeStage() {
        activity.progress = 0
        Trigger("subreddit-moderate", {})
    }

    function advance(time) {
        while (activity.progress + time > activity.time) {
            time -= (activity.time - activity.progress)
            completeStage()
        }
        activity.progress += time
    }

    const triggers = []
    onMount(() => {
        triggers.push(Trigger.on("tick", advance))
    })

    onDestroy(() => {
        triggers.forEach(trigger => trigger.cancel())
    })

</script>
