<script>
    export let max = 100
    export let current = 0
    export let caption = ""

    $: progress = (100 * current / max).toFixed(2)
</script>

<div class="bar" style="--progress:{progress}%">
    <div class="bg"></div>
    <div class="fg"></div>
    <div class="caption">{caption}</div>
</div>

<style>
    div.bar {
        width: 16em;
        height: 0.5em;
        padding: 10px 0;
        position: relative;
    }

    div.fg, div.bg, div.caption {
        position: absolute;
        left : 0;
        top : 0;
        height : 100%;
        width: 100%;
    }

    div.bg {
        background-color: #222222;
    }

    div.fg {
        background-color: #666666;
        width: var(--progress);
    }

    div.caption {
        display: flex;
        align-items: center;
        justify-content: center;
    }
</style>
