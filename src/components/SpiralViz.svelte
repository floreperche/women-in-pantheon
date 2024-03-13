<script>
  import Flower from "./Flower.svelte";
  export let data;
  export let width;
  export let height;
  export let marginHeight;

  // Spiral viz
  const pointsPerRotation = 20;
  let rank = 1;
  const inner = 30;
  const param = 0.38;

  $: data
    .sort((a, b) => a.transfered_date - b.transfered_date)
    .map((e) => {
      e["rank"] = rank;
      e["spiral_x"] =
        (inner + rank / param) *
        Math.cos((rank / pointsPerRotation) * 2 * Math.PI);
      e["spiral_y"] =
        (inner + rank / param) *
        Math.sin((rank / pointsPerRotation) * 2 * Math.PI);
      // processedData.push(e);
      rank++;
    });

  // console.log(processedData);

  // Final year annotation
  let annotationCoordinates = {
    spiral_x:
      (inner + (data.length + 1) / param) *
      Math.cos(((data.length + 1) / pointsPerRotation) * 2 * Math.PI),
    spiral_y:
      (inner + (data.length + 1) / param) *
      Math.sin(((data.length + 1) / pointsPerRotation) * 2 * Math.PI),
  };
</script>

<div class="graph-container spiral">
  {#if data}
    <!-- legend -->
    <div class="legend">
      <svg {width} height={marginHeight}>
        <g transform="translate({width / 2 - 200} 25), scale({0.7})"
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
        <g transform="translate({width / 2 - 100} 25), scale({0.7})"
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
        <g transform="translate({width / 2 + 150} 25), scale({0.7})">
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

    <svg {height} {width} viewBox="-{width / 2} -{height / 2} {width} {height}">
      <g>
        <text x="0" y="-10" class="annotations" fill="#EEE7EF"
          >{data[0].transfered_date}</text
        >
        <text
          x={annotationCoordinates.spiral_x}
          y={annotationCoordinates.spiral_y}
          class="annotations"
          fill="#EEE7EF">{data[data.length - 1].transfered_date}</text
        >
        {#each data as person}
          <g
            transform="translate({person.spiral_x} {person.spiral_y}), scale({0.8 -
              person.id / 150})"
            class="{person.name} flower"
          >
            <Flower {person} />
            <!-- {console.log(person)} -->
          </g>
        {/each}
      </g></svg
    >{/if}
</div>

<style>
  .graph-container {
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
</style>
