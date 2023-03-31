<script>
    export let state
    export let can = {}

    $: can.getBorn = !state.person

    $: can.createSubreddit = !state.subreddit && state.person && state.person.level >= 3
    $: can.hireModerators = state?.subreddit?.totalPosts >= 1000 && state?.subreddit?.users >= 50

    $: can.createDiscord = !state.discord && state.subreddit && state.subreddit.users >= 100
</script>
