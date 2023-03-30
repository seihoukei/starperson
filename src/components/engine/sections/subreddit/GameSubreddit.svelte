<script>
    import {onDestroy, onMount} from "svelte"
    import Trigger from "../../../../utility/trigger.js"

    export let state
    export let subreddit

    function updatePosts() {
        subreddit.totalPosts = subreddit.posts.reduce((v,x) => v+x)
        subreddit.totalQuality = subreddit.posts.reduce((v,x,i) => v+x*i)
        if (subreddit.totalPosts)
            subreddit.postQuality = subreddit.totalQuality / subreddit.totalPosts
        else
            subreddit.postQuality = 0
    }

    function createSubreddit() {
        subreddit = Object.create(null)
        subreddit.posts = [0,0,0,0,0]
        subreddit.rules = 0
        subreddit.moderators = 0
        subreddit.users = 0
        updatePosts()
    }

    const triggers = []
    onMount(() => {
        triggers.push(Trigger.on("subreddit-create", createSubreddit))
    })

    onDestroy(() => {
        triggers.forEach(trigger => trigger.cancel())
    })

</script>
