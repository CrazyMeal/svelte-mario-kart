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

  <div class="grid grid-rows-10 grid-flow-col h-full">

    <div class="row-span-1 col-span-2	">
      <div class="container mx-auto flex p-3">
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
    </div>
    <div class="row-span-5 col-span-2">
      <div class="container mx-auto flex flex-col overflow-y-auto content-start h-full p-3">
        test
        {#each results as result}
          <RankingLine {...result}/>
        {/each}
      </div>
    </div>
    <div class="row-span-6 col-span-8 p-8">Le graph</div>
  </div>

</main>
