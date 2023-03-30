<script>
    import Trigger from "../../utility/trigger.js"

    export let game

</script>

<div class="container">
    <button on:click={() => Trigger("reset-game")}>Reset game</button>
</div>

<style>
    div.container {
        display: flex;
        width: 100vw;

    }
</style>
