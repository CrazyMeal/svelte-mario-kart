<script>
  import Select from "svelte-select";

  import { tracks } from "./stores/readables/tracks.js";
  import { rankings } from "./stores/readables/rankings.js";

  import { trackResults } from "./stores/writables/track-results.js";

  let selectedTrack;
  let selectedRank;

  let results;
  trackResults.subscribe((value) => {
    console.log("Before value: ", value);
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
  .custom-input {
    border: var(--border, 1px solid #d8dbdf);
    border-radius: var(--borderRadius, 3px);
    height: var(--height, 42px);
    --padding: 0 16px;
    padding: var(--padding);
    position: relative;
    display: flex;
    align-items: center;
  }

  :global(.position-suffix) {
    font-size: 0.75em;
    vertical-align: top;
  }

  :global(.selectedItem) {
    overflow-y: hidden;
  }
</style>

<!-- Using https://github.com/rob-balfre/svelte-select -->



	
  <div class="container mx-auto flex justify-center p-8">
    <table class="table-auto">
      <thead>
        <tr>
          <th class="px-4 py-2">Course</th>
          <th class="px-4 py-2">Position</th>
          <th class="px-4 py-2">Points</th>
        </tr>
      </thead>
      <tbody>
        {#each results as result}
          <tr>
            <td class="border px-4 py-2">{result.track}</td>
            <td class="border px-4 py-2">{result.rank.value.key}</td>
            <td class="border px-4 py-2">{result.rank.value.points}</td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>

  <div class="container mx-auto flex p-8">
    <Select
      containerClasses="flex-1 m-3"
      items={$tracks}
      bind:selectedValue={selectedTrack}
      placeholder="Course" />
    <Select
      containerClasses="flex-1 m-3"
      items={rankings}
      bind:selectedValue={selectedRank}
      placeholder="Classement" />
    <button
      class="flex-1 m-3 bg-blue-500 hover:bg-blue-700 text-white font-bold
      rounded"
      on:click={addRandomResult}>
      Add
    </button>
  </div>

