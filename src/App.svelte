<script>

  import TracksChart from "./TracksChart.svelte";
  import TrackSelector from "./TrackSelector.svelte";
  import RankingBoard from "./RankingBoard.svelte";
  import Score from "./Score.svelte";

  Array.prototype.move = function (from, to) {
    this.splice(to, 0, this.splice(from, 1)[0]);
  };

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

<main class="h-screen">
  <div class="flex flex-col lg:flex-row h-full">
    <div class="flex flex-col flex-1">
      <TrackSelector />
      <RankingBoard />
    </div>

    <div class="flex flex-col flex-1">
      <Score />
      <TracksChart />
    </div>
  </div>
</main>
