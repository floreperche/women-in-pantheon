<script>
  export let data;
  export let width;

  const maxWidth = 180;
  const nudge = 20;
  $: xPosition =
    data.x + maxWidth + nudge > width
      ? data.x - maxWidth - nudge
      : data.x + nudge;
  $: console.log(xPosition);
</script>

<div class="tooltip" style="left: {xPosition}px; top: {data.y + 20}px">
  <h2>{data.data.name}</h2>
  <p>{data.data.birth_date}-{data.data.death_date}</p>
  {#if data.data.sex === "W"}Panthéonisée en {data.data.transfered_date}
  {:else}
    Panthéonisée en {data.data.transfered_date}
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
    font-size: 1rem;
    font-weight: 500;
    margin-bottom: 6px;
    width: 100%;
    display: flex;
    align-items: center;
  }

  .tooltip h2,
  .tooltip p {
    color: #120833;
  }
</style>
