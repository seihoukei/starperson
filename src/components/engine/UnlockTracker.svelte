<script>
    export let state
    export let can = {}

    $: can.getBorn = !state.person
    $: can.createSubreddit = !state.subreddit && state.person && state.person.level >= 3
    $: can.createDiscord = !state.discord && state.person && state.person.level >= 10
</script>
