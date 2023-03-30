<script>
    import UISectionPersonal from "./UISectionPersonal.svelte"
    import UISectionDiscord from "./UISectionDiscord.svelte"
    import UISectionSubreddit from "./UISectionSubreddit.svelte"
    import UIMeta from "./UIMeta.svelte"

    export let game
</script>

{#if import.meta.env.MODE === "development"}
    <pre class="debug">{JSON.stringify(game, null, 1).replace(/(\d+\.\d{1,2})\d+/g,"$1")}</pre>
{/if}

{#if game?.state}

    <UIMeta {game}/>
    <div class="content">
        {#if game.state.person || game.can?.getBorn}
            <UISectionPersonal {game}/>
        {/if}
        {#if game.state.subreddit || game.can?.createSubreddit}
            <UISectionSubreddit {game}/>
        {/if}
        {#if game.state.discord || game.can?.createDiscord}
            <UISectionDiscord {game}/>
        {/if}
    </div>
{/if}

<style>
    pre.debug {
        position: absolute;
        right : 0;
        top : 0;
        opacity: 0.5;
        pointer-events: none;
    }

    div.content {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        align-items: center;
        justify-content: center;
        min-height: 100vh;
        column-gap: 1em;
    }
</style>
