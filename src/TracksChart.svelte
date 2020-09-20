<script>
  import { onMount, afterUpdate, onDestroy } from "svelte";

  import { trackResults } from "./stores/writables/track-results.js";

  const defaultLabels = ['Start', '']; // Hacky init so lib doesn't throw a lot of warnings

  //  The Chart returned from frappe
  let chart = null;

  //  DOM node for frappe to latch onto
  let chartRef;

  let data = {
    labels: defaultLabels, // Hacky init so lib doesn't throw a lot of warnings
    datasets: [{
      chartType: "line",
      values: [0],
    }],
  };

  let results;
  trackResults.subscribe((value) => results = value);

  $: if (results) {
    let sum = 0;

    data = {
      labels: results.length === 0 ? defaultLabels : ["Start", ...results.map(result => result.track)],
      datasets: [{
        //chartType: "line",
        values: [0, ...results.map(result => sum = (sum || 0) + result.rank.value.points )],
      }]
    }
  }

  onMount(() => {
    chart = new frappe.Chart(chartRef, {
      // or a DOM element,
      // new Chart() in case of ES6 module with above usage
      data: data,
      type: "line", // or 'axis-mixed', 'bar', 'line', 'scatter', 'pie', 'percentage'
      height: 500,
      colors: ["#68D391"],
    });
  });
  //  Update the chart when incoming data changes
  afterUpdate(() => chart.update(data));

  //  Mark Chart references for garbage collection when component is unmounted
  onDestroy(() => {
    chart = null;
  });

</script>

<style>

</style>

<div class="flex-grow p-3" bind:this={chartRef} />
