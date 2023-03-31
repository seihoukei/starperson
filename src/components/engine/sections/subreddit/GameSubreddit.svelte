<script>
    import {onDestroy, onMount} from "svelte"
    import Trigger from "../../../../utility/trigger.js"

    export let state
    export let subreddit

    const TICK_TIME = 1

    function updatePosts() {
        subreddit.totalPosts = subreddit.posts.reduce((v,x) => v+x)
        subreddit.totalQuality = subreddit.posts.reduce((v,x,i) => v+x*i)
        if (subreddit.totalPosts)
            subreddit.postQuality = 1 + subreddit.totalQuality / subreddit.totalPosts
        else
            subreddit.postQuality = 0
    }

    function createSubreddit() {
        subreddit = Object.create(null)
        subreddit.posts = [0,0,0,0,0]
        subreddit.rules = 0 // todo: decrease post rate, increase quality
        subreddit.moderators = 0 // todo: auto moderate, can reduce users
        subreddit.users = 0
        updatePosts()
    }

    function publishPost(post) {
        subreddit.posts[post.quality] += 1
        updatePosts()
    }

    function processTick() {
        //todo: take post and user count into account
        if (Math.random() * 100 < subreddit.postQuality ** 3)
            subreddit.users += 1

        for (let quality = 0; quality <= 4; quality++) {
            const rate = ((5 - quality) / 5) ** 2
            const ceiling = subreddit.users * rate * Math.random()
            const amount = Math.floor(Math.random() ** 5 * ceiling)
            const bonus = (subreddit.users > 0 && Math.random() < 0.001 * (5 - quality)) ? 1 : 0
            subreddit.posts[quality] += amount + bonus
        }

        updatePosts()
    }

    let timeToTick = TICK_TIME
    function advance(time) {
        if (!subreddit)
            return
        timeToTick -= time
        while(timeToTick < 0) {
            timeToTick += TICK_TIME
            processTick()
        }
    }

    function moderate() {
        if (Math.random() < 0.5)
            subreddit.posts[0] = Math.floor(subreddit.posts[0] * (1 - Math.random() * 0.1))
        else
            subreddit.posts[0] = Math.max(0, subreddit.posts[0] - 1)

        if (subreddit.posts[0] < subreddit.posts[1]) {
            if (Math.random() < 0.5)
                subreddit.posts[1] = Math.floor(subreddit.posts[1] * (1 - Math.random() * 0.1))
            else
                subreddit.posts[1] = Math.max(0, subreddit.posts[1] - 1)
        }
        updatePosts()
    }

    const triggers = []
    onMount(() => {
        triggers.push(Trigger.on("subreddit-create", createSubreddit))
        triggers.push(Trigger.on("tick", advance))
        triggers.push(Trigger.on("subreddit-publish-post", publishPost))
        triggers.push(Trigger.on("subreddit-moderate", moderate))
    })

    onDestroy(() => {
        triggers.forEach(trigger => trigger.cancel())
    })

</script>
