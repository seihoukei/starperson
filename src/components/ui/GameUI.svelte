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
        left : 0;
        top : 20px;
        opacity: 0.5;
        pointer-events: none;
        font-size: 8px;
    }

    div.content {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        align-items: center;
        justify-content: center;
        column-gap: 1em;
        row-gap: 1em;
        overflow: auto;
    }
</style>
