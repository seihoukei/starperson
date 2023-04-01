<script>
    import UIActivityGame from "./UIActivityGame.svelte"
    import Trigger from "../../utility/trigger.js"

    export let game

    const MOODS = ["Depressed", "Sad", "Fine", "Optimistic", "Delighted", "Yes"]

    $: person = game?.state?.person
    $: mood = MOODS[Math.floor(person?.mood / 20)]

</script>

<div class="page">
    {#if person}
        <div class="flavor">
            You are a star person in an incremental game forest.
        </div>

        <div class="stats">
            <div class="stat">Mood : <span class="value">{mood}</span></div>
            <div class="stat">IG expertise : <span class="value">{person?.expertise?.toFixed(2)}</span></div>
            <div class="stat">Player level : <span class="value">{person?.level}</span> (next at <span class="value">{person?.nextLevelExpertise?.toFixed(2)}</span>)</div>
            <div class="stat">Games beaten : <span class="value">{person?.beatenGames}</span></div>
            <div class="stat">Games dropped : <span class="value">{person?.droppedGames}</span></div>
        </div>

        <UIActivityGame {game} />
        {#if game?.can?.createSubreddit}
            <button on:click={() => Trigger("subreddit-create")}>Create subreddit</button>
        {/if}
        {#if game?.can?.createDiscord}
            <button on:click={() => Trigger("discord-create")}>Create Discord server</button>
        {/if}
    {:else if game?.can?.getBorn}
        <button on:click={() => Trigger("person-get-born")}>Get born</button>
    {/if}

</div>

<style>

</style>
