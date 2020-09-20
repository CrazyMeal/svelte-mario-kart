<script>
  import RankingLine from "./RankingLine.svelte";

  import { trackResults } from "./stores/writables/track-results.js";

  let results;

  trackResults.subscribe((value) => results = value);

  function removeResult(event) {
    trackResults.update((results) =>
      results.filter((result) => result.id !== event.detail.id)
    );
  }

  function pushDown(event) {
    const indexOfResultToMove = results
      .map((result) => result.id)
      .indexOf(event.detail.id);

    if (indexOfResultToMove !== -1 && indexOfResultToMove !== 0) {
      trackResults.update((results) => {
        results.move(indexOfResultToMove, indexOfResultToMove - 1);
        return results;
      });
    }
  }

  function pushUp(event) {
    const indexOfResultToMove = results
      .map((result) => result.id)
      .indexOf(event.detail.id);

    if (
      indexOfResultToMove !== -1 &&
      indexOfResultToMove + 1 !== results.length
    ) {
      trackResults.update((results) => {
        results.move(indexOfResultToMove, indexOfResultToMove + 1);
        return results;
      });
    }
  }
</script>

<style>

</style>

<div
  class="flex flex-col-reverse container h-full p-3 overflow-y-auto justify-end">
  {#each results as result}
    <RankingLine
      {...result}
      on:remove={removeResult}
      on:pushUp={pushUp}
      on:pushDown={pushDown} />
  {/each}
</div>
