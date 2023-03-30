<script>
    import SaveStateManager from "./SaveStateManager.svelte"
    import UnlockTracker from "./UnlockTracker.svelte"
    import GamePerson from "./sections/person/GamePerson.svelte"
    import GameDiscord from "./sections/discord/GameDiscord.svelte"
    import GameSubreddit from "./sections/subreddit/GameSubreddit.svelte"
    import GameActivities from "./activities/GameActivities.svelte"

    export let game = {}

    let can, state

    $: game.state = state
    $: game.can = can

</script>

<SaveStateManager bind:state />

{#if state}
    <UnlockTracker {state} bind:can />
    <GamePerson {state} bind:person={state.person} />
    <GameSubreddit {state} bind:subreddit={state.subreddit} />
    <GameDiscord {state} bind:discord={state.discord} />
    <GameActivities {state} bind:activity={state.activity} bind:activities={state.activities} />
{/if}
