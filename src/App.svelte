<script>
  import data from "./assets/pantheon_data.json";

  const margin = { top: 10, right: 100, left: 100, bottom: 30 };

  let width = 500;
  let height = 500;

  // Spiral viz
  const radius = width / 2 - 20;
  // Number of spirals
  const coils = 5.6;
  const rotation = 2 * Math.PI;
  const thetaMax = coils * 1.5 * Math.PI;
  const awayStep = radius / thetaMax;
  // Spacing
  const chord = 36;
  let theta = chord / awayStep;
  // let initialData = data.sort((a, b) => a.grade - b.grade);
  $: processedData = [];
  $: data
    .sort((a, b) => a.transfered_date - b.transfered_date)
    .map((e) => {
      if (theta <= thetaMax) {
        let away = awayStep * theta;
        let around = theta + rotation;
        let x = width / 2 + Math.cos(around) * away;
        let y = height / 2 + Math.sin(around) * away;
        theta += chord / away;
        e["x"] = x;
        e["y"] = y;
        processedData.push(e);
      }
    });
</script>

<main>
  <div class="intro">
    <h1>Aux grandes femmes, la Nation reconnaissante?</h1>
    <p>Dataviz</p>
  </div>

  <div class="graph-container second">
    <svg {height} {width}>
      <g>
        {#if processedData}
          {#each processedData as person}
            <circle
              cx={person.x}
              cy={person.y}
              r="16"
              fill={person.sex === "M" ? "#1B2B5C" : "#F34C63"}
            />{/each}
        {/if}</g
      ></svg
    >
  </div>
</main>

<style>
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
