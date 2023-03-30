<script>
    import {onDestroy, onMount} from "svelte"
    import SaveProcessor from "../../utility/save-processor.js"
    import Trigger from "../../utility/trigger.js"

    export let state = Object.create(null)

    const AUTOSAVE_INTERVAL = 10000
    const AUTOSAVE_SLOT = "StarPerson_Save__Autosave"

    const DEFAULT_GAME_STATE = {
        person : null,
        subreddit : null,
        discord : null,
    }

    function saveGame(slot = AUTOSAVE_SLOT) {
        const saveData = SaveProcessor.encode(state)
        localStorage[slot] = saveData
    }

    function loadGame(slot = AUTOSAVE_SLOT) {
        const saveData = localStorage[slot]
        resetGame()
        Object.assign(state, SaveProcessor.decode(saveData))
    }

    function resetGame() {
        state = Object.create(null)
        Object.assign(state, DEFAULT_GAME_STATE)
    }

    let interval = null
    onMount(() => {
        loadGame()
        interval = setInterval(saveGame, AUTOSAVE_INTERVAL)
    })

    onDestroy(() => {
        clearInterval(interval)
    })

    const triggers = []
    onMount(() => {
        triggers.push(Trigger.on("save-game", (slot) => saveGame(slot)))
        triggers.push(Trigger.on("load-game", (slot) => loadGame(slot)))
        triggers.push(Trigger.on("reset-game", resetGame))
    })

    onDestroy(() => {
        triggers.forEach(trigger => trigger.cancel())
    })

</script>
