<script>
  import { scaleOrdinal } from "d3";
  import { tweened } from "svelte/motion";
  export let data;
  export let width;
  import { fly, fade } from "svelte/transition";

  // Color scale
  const colorRange = ["#F34C63", "#7480D2"];
  let colorScale = scaleOrdinal().domain(["W", "M"]).range(colorRange);

  // Managing overlap
  const maxWidth = 180;
  const nudge = 20;
  $: xPosition =
    data.x + maxWidth + nudge > width
      ? data.x - maxWidth - nudge
      : data.x + nudge;
</script>

<!-- Tooltip -->
<div
  class="tooltip"
  style="left: {xPosition}px; top: {data.y + 20}px"
  in:fly={{ y: 10, duration: 200, delay: 200 }}
  out:fade
>
  <h3>
    {data.data.name} ({data.data.birth_date}-{data.data.death_date})
  </h3>
  <!-- Display is Panthéonisé -->
  {#if data.data.status === "Panthéonisée"}
    <p>
      {#if data.data.sex === "W"}Panthéonisée en <span
          style="background-color : {colorScale(data.data.sex)}"
          >{data.data.transfered_date}</span
        >
      {:else}
        Panthéonisé en <span
          style="background-color : {colorScale(data.data.sex)}"
          >{data.data.transfered_date}</span
        >
      {/if}
    </p>
    <!-- Display is not Panthéonisé -->
  {:else}
    <p>
      Repose au Panthéon depuis <span
        style="background-color : {colorScale(data.data.sex)}"
        >{data.data.transfered_date}</span
      >, mais n'est pas panthéonisé(e)
    </p>
  {/if}
</div>

<style>
  .tooltip {
    position: absolute;
    padding: 8px 6px;
    background: #eee7ef;
    border-radius: 3px;
    color: #120833;
    pointer-events: none;
    transition:
      top 300ms ease,
      left 300ms ease;
    max-width: 180px;
  }

  .tooltip h3,
  .tooltip h3 span,
  p {
    color: #120833;
  }

  p span {
    color: #eee7ef;
    padding: 2px 4px;
    width: fit-content;
    font-weight: 400;
    font-size: 14px;
    border-radius: 4px;
  }
</style>
