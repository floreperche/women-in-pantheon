<script>
  export let data;
  export let width;
  export let colorScale;

  import { fly, fade } from "svelte/transition";

  const maxWidth = 180;
  const nudge = 20;
  $: xPosition =
    data.x + maxWidth + nudge > width
      ? data.x - maxWidth - nudge
      : data.x + nudge;
  // $: console.log(xPosition);
</script>

<div
  class="tooltip"
  style="left: {xPosition}px; top: {data.y + 20}px"
  in:fly={{ y: 10, duration: 200, delay: 200 }}
  out:fade
>
  <h2>
    {data.data.name} ({data.data.birth_date}-{data.data.death_date})
  </h2>
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

  h2 {
    font-size: 0.9rem;
    font-weight: 600;
    margin-bottom: 6px;
    width: 100%;
  }

  .tooltip h2,
  .tooltip h2 span,
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
