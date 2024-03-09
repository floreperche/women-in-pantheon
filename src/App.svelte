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

  // flowers
  let petals = [];
  petals.length = 9;
</script>

<main>
  <div class="intro">
    <h1>Aux grandes femmes, la Nation reconnaissante?</h1>
    <p>Dataviz</p>
  </div>

  <div class="graph-container">
    <svg {height} {width}>
      <g>
        {#if processedData}
          {#each processedData as person}
            <g transform="translate({person.x} {person.y}), scale(0.5)">
              {#each petals as petal, i}
                <path
                  fill={person.sex === "M" ? "#1B2B5C" : "#F34C63"}
                  d="M20.7,2.5C20.7,15.4,10.4,30.8,2.4,30.8C-5.7,30.8,-11.3,15.4,-11.3,2.5C-11.3,-10.3,-5.7,-20.7,2.4,-20.7C10.4,-20.7,20.7,-10.3,20.7,2.5Z"
                  transform="rotate({80 * i})"
                  fill-opacity="50%"
                />
              {/each}

              <path
                fill={person.sex === "M" ? "#121c3b" : "#bd2d41"}
                d="M48.1,-66.9C63,-55.4,76.2,-42.2,82.6,-26C89,-9.8,88.5,9.4,81.8,25.5C75,41.5,62,54.4,47.3,62.6C32.6,70.8,16.3,74.3,0.2,74C-15.9,73.7,-31.7,69.6,-43.7,60.6C-55.7,51.5,-63.9,37.4,-71,21.6C-78.2,5.8,-84.4,-11.8,-81.9,-28.7C-79.3,-45.6,-68,-61.8,-52.9,-73.2C-37.8,-84.6,-18.9,-91.3,-1.1,-89.7C16.6,-88.2,33.2,-78.4,48.1,-66.9Z"
                transform="scale(0.1)"
              />
            </g>
          {/each}
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
