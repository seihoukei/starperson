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
        subreddit.moderators = 0
        subreddit.users = 0
        updatePosts()
    }

    function publishPost(post) {
        subreddit.posts[post.quality] += 1
        updatePosts()
    }

    function populate() {
        if (Math.random() * 100 < subreddit.postQuality ** 3)
            subreddit.users += 1

        subreddit.users += Math.floor(subreddit.postQuality * (Math.random() ** 2) * (subreddit.totalPosts ** 0.2))
        subreddit.users -= Math.floor(subreddit.moderators * Math.random() ** 2)
        subreddit.users = Math.floor(subreddit.users * Math.min(1, (subreddit.postQuality - 3) / 3 + 1))

        subreddit.users = Math.max(0, subreddit.users)
    }

    function createUserPosts() {
        for (let quality = 0; quality <= 4; quality++) {
            const rate = 1 / (2 + 0.25 * quality)
            const ceiling = subreddit.users ** rate
            const amount = Math.floor(2 * Math.random() ** (quality/2 + 1) * ceiling)
            const bonus = (subreddit.users > 0 && Math.random() < 0.001 * (5 - quality)) ? 1 : 0
            subreddit.posts[quality] += amount + bonus
        }

        updatePosts()
    }

    function processTick() {
        //todo: take post and user count into account
        populate()
        createUserPosts()
    }

    let timeToTick = TICK_TIME
    function advance(time) {
        if (!subreddit)
            return
        timeToTick -= time
        while(timeToTick < 0) {
            timeToTick += TICK_TIME
            processTick()
            for (let i = 0; i < subreddit.moderators; i++) {
                if (Math.random() > 0.5)
                    moderate()
            }
        }
    }

    function moderate({person = false} = {}) {
        const moderatedPosts = [0,0,0,0,0]

        if (Math.random() < 0.5)
            moderatedPosts[0] = Math.ceil(subreddit.posts[0] * (Math.random() * 0.1))
        else
            moderatedPosts[0] = Math.min(subreddit.posts[0], 1)

        if (subreddit.posts[0] < subreddit.posts[1]) {
            if (Math.random() < 0.5)
                moderatedPosts[1] = Math.ceil(subreddit.posts[1] * (Math.random() * 0.1))
            else
                moderatedPosts[1] = Math.min(subreddit.posts[1], 1)
        }

        let amount = 0
        for (let quality = 0; quality <= 4; quality++) {
            amount += moderatedPosts[quality]
            subreddit.posts[quality] -= moderatedPosts[quality]
        }

        updatePosts()

        if (person) {
            Trigger("person-moderate", {amount})
        }
    }

    function hireModerator() {
        subreddit.users -= 10
        subreddit.moderators += 1
    }

    const triggers = []
    onMount(() => {
        triggers.push(Trigger.on("subreddit-create", createSubreddit))
        triggers.push(Trigger.on("tick", advance))
        triggers.push(Trigger.on("subreddit-publish-post", publishPost))
        triggers.push(Trigger.on("subreddit-moderate", moderate))
        triggers.push(Trigger.on("subreddit-hire-moderator", hireModerator))
    })

    onDestroy(() => {
        triggers.forEach(trigger => trigger.cancel())
    })

</script>
