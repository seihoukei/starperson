<script>
    import {onDestroy, onMount} from "svelte"
    import Trigger from "../../../../utility/trigger.js"

    export let state
    export let discord

    function createDiscord() {
        discord = Object.create(null)
        discord.name = Math.random()
    }

    const triggers = []
    onMount(() => {
        triggers.push(Trigger.on("create-discord", createDiscord))
    })

    onDestroy(() => {
        triggers.forEach(trigger => trigger.cancel())
    })

</script>

{#if discord}
{/if}
