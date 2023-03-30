<script>
    import {onDestroy, onMount} from "svelte"
    import Trigger from "../../../utility/trigger.js"

    export let state
    export let activity = {}

    if (activity.stage === undefined) {
        newPost()
    }

    function newPost() {
        activity.stage = Math.floor(Math.random() ** 3 * 4)
        activity.completed = false
        activity.time = 2 + Math.random() * 2 * 2 ** activity.stage
        activity.totalTime = activity.time
        activity.progress = 0
    }

    function completeStage() {
        activity.progress = 0
        activity.completed = true
        if (activity.stage >= 3) {
            publish()
        } else {
            activity.stage += 1
            activity.time *= 2 + Math.random() * activity.stage / 2
        }
    }

    function advance(time) {
        while (activity.progress + time > activity.time) {
            time -= (activity.time - activity.progress)
            completeStage()
        }
        activity.progress += time
    }

    function publish() {
        if (activity.completed) {
            Trigger("subreddit-publish-post", {
                quality : activity.stage
            })
        }
        newPost()
    }

    const triggers = []
    onMount(() => {
        triggers.push(Trigger.on("tick", advance))
        triggers.push(Trigger.on("activity-post-publish", publish))
    })

    onDestroy(() => {
        triggers.forEach(trigger => trigger.cancel())
    })

</script>
