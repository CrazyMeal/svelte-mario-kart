<script>
  import { v4 as uuidv4 } from "uuid";
  import Select from "svelte-select";

  import { tracks } from "./stores/readables/tracks.js";
  import { rankings } from "./stores/readables/rankings.js";
  import { trackResults } from "./stores/writables/track-results.js";

  let selectedTrack;
  let selectedRank;

  function addResult() {
    if (selectedTrack && selectedRank) {
      trackResults.update((results) => [
        ...results,
        { id: uuidv4(), track: selectedTrack.value, rank: selectedRank },
      ]);

      selectedTrack = null;
      selectedRank = null;

      window.resizeBy(0, 500);
    }
  }
</script>

<style>

</style>

<div class="flex p-3">
  <Select
    containerClasses="flex-1 m-3"
    isClearable="false"
    items={$tracks}
    bind:selectedValue={selectedTrack}
    placeholder="Course" />
  <Select
    containerClasses="flex-1 m-3"
    isClearable="false"
    items={rankings}
    bind:selectedValue={selectedRank}
    placeholder="Classement" />
  <button
    class="flex-1 m-3 bg-teal-400 text-white font-bold rounded"
    on:click={addResult}>
    Ajouter
  </button>
</div>
