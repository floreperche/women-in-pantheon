<script>
  import data from "./assets/pantheon_data.json";
  import Flower from "./components/Flower.svelte";

  const margin = { top: 10, right: 100, left: 100, bottom: 30 };

  let width = 800;
  let height = 800;

  // Spiral viz
  $: processedData = [];
  const pointsPerRotation = 20;
  let rank = 1;
  const inner = 30;
  const param = 0.38;

  $: data
    .sort((a, b) => a.transfered_date - b.transfered_date)
    .map((e) => {
      e["rank"] = rank;
      e["x"] =
        (inner + rank / param) *
        Math.cos((rank / pointsPerRotation) * 2 * Math.PI);
      e["y"] =
        (inner + rank / param) *
        Math.sin((rank / pointsPerRotation) * 2 * Math.PI);
      // console.log(data);
      processedData.push(e);
      // console.log(processedData);
      rank++;
    });
</script>

<main>
  <div class="intro">
    <h1>Aux grandes femmes, la Nation reconnaissante?</h1>
    <p>Dataviz</p>
  </div>

  <div class="graph-container">
    <svg {height} {width} viewBox="-{width / 2} -{height / 2} {width} {height}">
      <g>
        {#if processedData}
          {#each processedData as person}
            <g
              transform="translate({person.x} {person.y}), scale({0.8 -
                person.id / 150})"
              class="{person.name} flower"
            >
              <Flower {person} />
            </g>
          {/each}
        {/if}</g
      ></svg
    >
  </div>
</main>

<style>
  main {
    background-color: #120833;
  }
  .intro {
    font-family: "Cabinet Grotesk", sans-serif;
    text-align: center;
  }

  .graph-container {
    margin: 0 auto;
    display: flex;
    justify-content: center;
  }
</style>
