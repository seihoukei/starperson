<script>
    import Trigger from "../../utility/trigger.js"

    export let game

    $: subreddit = game?.state?.subreddit

</script>

<div class="page">
    {#if subreddit}
        <div class="flavor">
            The chat is dead. The room is cool.
        </div>

        <div class="stats">
        </div>

    {:else if game?.can?.createSubreddit}
        <button on:click={() => Trigger("subreddit-create")}>Create subreddit</button>
    {/if}

</div>

<style>

</style>
