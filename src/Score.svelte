<script>
  import { trackResults } from "./stores/writables/track-results.js";
  
  let score;
  trackResults.subscribe((value) => {
    score = value
      .map((result) => result.rank.value.points)
      .reduce(
        (previousScore, currentPoints) => previousScore + currentPoints,
        0
      );
  });
</script>

<div class="container font-bold pt-8 p-3">Score: {score}</div>