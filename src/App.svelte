<script>
  import Select from "svelte-select";
  import RankingLine from "./RankingLine.svelte";

  import { tracks } from "./stores/readables/tracks.js";
  import { rankings } from "./stores/readables/rankings.js";

  import { trackResults } from "./stores/writables/track-results.js";

  let selectedTrack;
  let selectedRank;

  let results;
  trackResults.subscribe((value) => {
    results = value;
  });

  function addRandomResult() {
    trackResults.update((results) => [
      ...results,
      { track: selectedTrack.value, rank: selectedRank },
    ]);
  }
</script>

<style>
  :global(.position-suffix) {
    font-size: 0.75em;
    vertical-align: top;
  }

  :global(.selectedItem) {
    overflow-y: hidden;
  }
</style>

<!-- Using https://github.com/rob-balfre/svelte-select -->

<main class="h-screen bg-gray-200">

  <div class="flex flex-row h-full">
    <div class="flex flex-col flex-1">
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
          on:click={addRandomResult}>
          Add
        </button>
      </div>
      
      <div class="flex flex-col container h-full p-3 overflow-y-auto content-start">
        {#each results as result}
          <RankingLine {...result} />
        {/each}
      </div>
    </div>

    <div class="flex flex-1">Le graph</div>
  </div>

</main>
