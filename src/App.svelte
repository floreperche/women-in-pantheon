<script>
  import data from "./assets/pantheon_data.json";
  import Flower from "./components/Flower.svelte";

  const margin = { top: 10, right: 100, left: 100, bottom: 30 };

  let width = 600;
  let height = 500;
  let marginHeight = 150;

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

  // Final year annotation
  let annotationCoordinates = {
    x:
      (inner + (data.length + 1) / param) *
      Math.cos(((data.length + 1) / pointsPerRotation) * 2 * Math.PI),
    y:
      (inner + (data.length + 1) / param) *
      Math.sin(((data.length + 1) / pointsPerRotation) * 2 * Math.PI),
  };
</script>

<main>
  <div class="intro">
    <h1>Aux grandes femmes, la Nation reconnaissante?</h1>
    <p>Dataviz</p>
  </div>

  <div class="graph-container">
    {#if processedData}
      <div class="legend">
        <svg {width} height={marginHeight}>
          <g
            transform="translate({width / 2 - 200} {marginHeight / 2 -
              10}), scale({0.7})"
            ><Flower person={{ sex: "W", status: "Panthéonisée" }} /><text
              fill="#EEE7EF"
              x="0"
              y="60"
              text-anchor="middle"
              transform="scale({1})"
              ><tspan x="0" dy="-0.5em" text-anchor="middle">Femme</tspan><tspan
                x="0"
                dy="1.2em"
                text-anchor="middle">panthéonisée</tspan
              >
            </text></g
          >
          <g
            transform="translate({width / 2 - 100} {marginHeight / 2 -
              10}), scale({0.7})"
            ><Flower person={{ sex: "M", status: "Panthéonisée" }} /><text
              fill="#EEE7EF"
              x="0"
              y="60"
              text-anchor="middle"
              transform="scale({1})"
              ><tspan x="0" dy="-0.5em" text-anchor="middle">Homme</tspan><tspan
                x="0"
                dy="1.2em"
                text-anchor="middle">panthéonisé</tspan
              ></text
            ></g
          >
          <g
            transform="translate({width / 2 + 150} {marginHeight / 2 -
              10}), scale({0.7})"
          >
            <g transform="translate(-60 0)">
              <Flower person={{ sex: "W", status: "Partenaire" }} />
            </g>

            <g transform="translate(60 0)">
              <Flower person={{ sex: "M", status: "Partenaire" }} /></g
            >
            <text
              fill="#EEE7EF"
              x="0"
              y="60"
              text-anchor="middle"
              transform="scale({1})"
            >
              <tspan x="0" dy="-0.5em" text-anchor="middle"
                >Femme / homme reposant au Panthéon,</tspan
              ><tspan x="0" dy="1.2em" text-anchor="middle"
                >sans être panthéonisé(e)</tspan
              >
            </text></g
          >
        </svg>
      </div>

      <svg
        {height}
        {width}
        viewBox="-{width / 2} -{height / 2} {width} {height}"
      >
        <g>
          <text x="0" y="-10" class="annotations" fill="#EEE7EF"
            >{processedData[0].transfered_date}</text
          >
          <text
            x={annotationCoordinates.x}
            y={annotationCoordinates.y}
            class="annotations"
            fill="#EEE7EF"
            >{processedData[processedData.length - 1].transfered_date}</text
          >
          {#each processedData as person}
            <g
              transform="translate({person.x} {person.y}), scale({0.8 -
                person.id / 150})"
              class="{person.name} flower"
            >
              <Flower {person} />
              {console.log(person)}
            </g>
          {/each}
        </g></svg
      >{/if}
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
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
</style>
