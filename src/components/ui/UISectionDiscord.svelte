<script>
    import Trigger from "../../utility/trigger.js"

    export let game

    $: discord = game?.state?.discord

</script>

{#if discord}
    <div class="page">
        <div class="flavor">
            A green shimmer erupts then disappears, and you hit the "Create server" button.
        </div>

        I'm sorry, that's it. Thanks for playing.

    </div>
{/if}

<style>

</style>
