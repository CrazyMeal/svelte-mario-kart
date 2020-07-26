<script>
    import { createEventDispatcher } from 'svelte';

    export let id;
    export let track;
    export let rank;

    const dispatch = createEventDispatcher();

    function removeResult(id) {
        dispatch('remove', { id: id });
    }

    function pushUp(id) {
        dispatch('pushUp', { id: id });
    }

    function pushDown(id) {
        dispatch('pushDown', { id: id });
    }

</script>

<style>
</style>

<div id={id} class="flex mb-1 mt-1 p-1 bg-teal-400 text-white font-bold shadow-md">
    <div class="order-arrows flex flex-col">
        <button on:click={ pushUp(id) }><span class="material-icons">keyboard_arrow_up</span></button>
        <button on:click={ pushDown(id) }><span class="material-icons">keyboard_arrow_down</span></button>
    </div>
    <div class="result-info flex content-center justify-center w-full p-3">
        {track} -  {@html rank.label} - {rank.value.points} points
    </div>
    <div>
        <button on:click={removeResult(id)}><span class="material-icons">clear</span></button>
    </div>
</div>