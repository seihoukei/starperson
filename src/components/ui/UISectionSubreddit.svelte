<script>
    import Trigger from "../../utility/trigger.js"
    import ActivityButton from "../ActivityButton.svelte"
    import UIActivityWritePost from "./UIActivityWritePost.svelte"
    import UIActivityModerate from "./UIActivityModerate.svelte"

    export let game

    $: subreddit = game?.state?.subreddit

</script>

{#if subreddit}
    <div class="page">
        <div class="flavor">
            The people are roaring. The posts are hot.
        </div>

        <div class="stats">
            <div class="stat">Users : <span class="value">{subreddit.users}</span></div>
            <div class="stat">
                Moderators : <span class="value">{subreddit.moderators}</span>
                {#if game?.can?.hireModerators}
                    <button on:click={() => Trigger("subreddit-hire-moderator")}>Hire moderator (-10 users)</button>
                {/if}
            </div>
            <div class="stat">Avg. post quality : <span class="value">{subreddit.postQuality.toFixed(2)}</span></div>
        </div>

        <UIActivityWritePost {game}/>

        <div class="stats">
            <div class="stat">Awesome posts : <span class="value">{subreddit.posts[4]}</span></div>
            <div class="stat">Cool posts : <span class="value">{subreddit.posts[3]}</span></div>
            <div class="stat">Interesting posts : <span class="value">{subreddit.posts[2]}</span></div>
            <div class="stat">Boring posts : <span class="value">{subreddit.posts[1]}</span></div>
            <div class="stat">Annoying posts : <span class="value">{subreddit.posts[0]}</span></div>
        </div>

        <UIActivityModerate {game}/>

    </div>
{/if}

<style>

</style>
