<script>
    import {onDestroy, onMount} from "svelte"
    import Trigger from "../../../utility/trigger.js"
    import WordList from "../../../dictionary/word-list.js"

    export let state
    export let activity = {}

    const POSTFIXES = ["Idle", "Incremental", "Clicker", "Game", "Simulator", "Quest", "Heroes", "Dimensions", "Tree"]

    if (activity.stage === undefined) {
        newGame()
    }


    function generateTitle() {
        const base = WordList[Math.floor(Math.random() * WordList.length)]
        const postfixes = [WordList[Math.floor(Math.random() * WordList.length)], ...POSTFIXES]
        const postfix = postfixes[Math.floor(postfixes.length * (Math.random() ** 2))]
        return `${base} ${postfix}`.replace(/\b(\S)/g, s => s.toUpperCase())
    }

    function newGame() {
        activity.title = generateTitle()
        activity.stage = 0
        activity.time = 2
        activity.totalTime = activity.time
        activity.progress = 0
    }

    function completeStage() {
        Trigger("person-beat-game-stage", {
            level : activity.stage,
            time : activity.time,
        })

        const nextStageChance = 1 / (activity.stage + 1)
        const hasNextStage = Math.random() < nextStageChance
        if (hasNextStage) {
            activity.stage += 1
            activity.progress = 0
            activity.time *= (1 + activity.stage + Math.random())
            activity.totalTime += activity.time
        } else {
            Trigger("person-beat-the-game", {
                levels : activity.stage,
                time : activity.totalTime,
            })
            newGame()
        }
    }

    function advance(time) {
        while (activity.progress + time > activity.time) {
            time -= (activity.time - activity.progress)
            completeStage()
        }
        activity.progress += time
    }

    function dropGame() {
        Trigger("person-dropped-the-game")
        newGame()
    }

    const triggers = []
    onMount(() => {
        triggers.push(Trigger.on("activity-advance", advance))
        triggers.push(Trigger.on("activity-game-drop", dropGame))
    })

    onDestroy(() => {
        triggers.forEach(trigger => trigger.cancel())
    })

</script>
