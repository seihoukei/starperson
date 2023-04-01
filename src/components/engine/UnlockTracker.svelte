<script>
    export let state
    export let can = {}

    $: can.getBorn = !state.person

    $: can.createSubreddit = !state.subreddit && state.person && state.person.level >= 3
    $: can.hireModerators = state?.subreddit?.users >= Math.max(15, 5 ** (state?.subreddit?.moderators + 1))

    $: can.createDiscord = !state.discord && state.subreddit && state.subreddit.users >= 10000
</script>
