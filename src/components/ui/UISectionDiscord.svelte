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

        <a href="https://discord.gg/cQ9pgrDVnJ">There.</a>

    </div>
{/if}

<style>

</style>
