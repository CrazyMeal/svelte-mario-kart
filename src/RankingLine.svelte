<script>
    export let track;
    export let rank;
</script>

<style>
</style>

<div class="container flex bg-teal-400 text-white font-bold shadow-md mb-1 mt-1 p-1">
    <div class="order-arrows flex flex-col">
        <span class="material-icons">keyboard_arrow_up</span>
        <span class="material-icons">keyboard_arrow_down</span>
    </div>
    <div class="result-info flex content-center justify-center w-full p-3">
        {track} - {rank.value.key} - {rank.value.points}
    </div>
    <div>
        <span class="material-icons">clear</span>
    </div>
</div>