<script>
    import {onDestroy, onMount} from "svelte"
    import Trigger from "../../../../utility/trigger.js"

    export let state
    export let person

    function updateLevel() {
        person.level = Math.max(0, Math.floor(Math.log2(person.expertise / 5)))
        person.nextLevelExpertise = 2 ** (person.level + 1) * 5
    }

    function getBorn() {
        person = Object.create(null)
        person.expertise = 0
        person.beatenGames = 0
        person.droppedGames = 0
        person.mood = 50
        person.speed = 1
        updateLevel()
    }

    function gainExp(exp) {
        person.expertise += exp
        updateLevel()
    }

    function gainMood(mood) {
        person.mood += mood
        person.mood = Math.max(0, Math.min(100, person.mood))
        person.speed = Math.min(1.25, 2 ** (person.mood / 25 - 2))
    }

    function beatGame(game) {
        person.beatenGames = (person.beatenGames ?? 0) + 1
        gainMood((game.levels + 1) * 1)
        const exp = (game.levels + 1) * (game.time) ** 0.5
        gainExp(exp)
    }

    function beatGameStage(stage) {
        const exp = (stage.level + 1) * (stage.time - 1) ** 0.5
        gainMood(0.5 * (stage.level + 1))
        gainExp(exp)
    }

    function dropGame() {
        person.droppedGames = (person.droppedGames ?? 0) + 1
        gainMood(-2)
    }

    function advance(time) {
        if (person)
            Trigger("activity-advance", person.speed * time, person.level)
    }

    function moderate({amount = 0}) {
        gainMood(-Math.min(10, amount / 10))
    }

    const triggers = []
    onMount(() => {
        triggers.push(Trigger.on("tick", advance))
        triggers.push(Trigger.on("person-get-born", getBorn))
        triggers.push(Trigger.on("person-beat-game-stage", beatGameStage))
        triggers.push(Trigger.on("person-beat-the-game", beatGame))
        triggers.push(Trigger.on("person-dropped-the-game", dropGame))
        triggers.push(Trigger.on("person-moderate", moderate))
    })

    onDestroy(() => {
        triggers.forEach(trigger => trigger.cancel())
    })

</script>

{#if person}
{/if}
