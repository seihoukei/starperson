<script>
    import Trigger from "../../utility/trigger.js"
    import ActivityButton from "../ActivityButton.svelte"

    export let game

    $: subreddit = game?.state?.subreddit

</script>

<div class="page">
    {#if subreddit}
        <div class="flavor">
            The chat is dead. The room is cool.
        </div>

        <div class="stats">
            <div class="stat">Users : <span class="value">{subreddit.users}</span></div>
            <div class="stat">Avg. post quality : <span class="value">{subreddit.postQuality}</span></div>
        </div>

        <ActivityButton activity="write-posts" {game}> Write posts </ActivityButton>

        <div class="stats">
            <div class="stat">Awesome posts : <span class="value">{subreddit.posts[4]}</span></div>
            <div class="stat">Cool posts : <span class="value">{subreddit.posts[3]}</span></div>
            <div class="stat">Interesting posts : <span class="value">{subreddit.posts[2]}</span></div>
            <div class="stat">Boring posts : <span class="value">{subreddit.posts[1]}</span></div>
            <div class="stat">Annoying posts : <span class="value">{subreddit.posts[0]}</span></div>
        </div>

        <ActivityButton activity="moderate-subreddit" {game}> Moderate subreddit </ActivityButton>

    {:else if game?.can?.createSubreddit}
        <button on:click={() => Trigger("subreddit-create")}>Create subreddit</button>
    {/if}

</div>

<style>

</style>
