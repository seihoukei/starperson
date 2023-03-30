<script>
    import GameEngine from "./components/engine/GameEngine.svelte"
    import GameUI from "./components/ui/GameUI.svelte"
    import GameTimer from "./components/GameTimer.svelte"

    let game = {}
</script>

<GameEngine bind:game />
<GameUI {game}/>

<GameTimer rate=60 boost=1 event="tick" />
