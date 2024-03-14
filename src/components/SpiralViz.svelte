<script>
  import Flower from "./Flower.svelte";
  import Tooltip from "./Tooltip.svelte";
  import { scaleOrdinal } from "d3";
  export let data;
  export let width;
  export let height;
  export let marginHeight;

  // Manage spiral shape
  const pointsPerRotation = 20;
  let rank = 1;
  const inner = 30;
  const param = 0.4;

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

  // Final year annotation
  let annotationCoordinates = {
    spiral_x:
      (inner + (data.length + 1) / param) *
      Math.cos(((data.length + 1) / pointsPerRotation) * 2 * Math.PI),
    spiral_y:
      (inner + (data.length + 1) / param) *
      Math.sin(((data.length + 1) / pointsPerRotation) * 2 * Math.PI),
  };

  // Tooltip
  let hovered = null;
</script>

<!-- Left elements -->
<div class="left">
  <h2 class="subtitle">
    Overview of all the persons transfered in Pantheon, since the French
    Revolution
  </h2>
  <div class="intro-viz">
    Lorem ipsum dolor sit amet consectetur. Interdum pellentesque proin duis
    accumsan rhoncus proin in eget viverra. Malesuada duis amet proin mauris
    netus fames. Auctor aliquam enim mollis placerat lorem magna cursus. Nunc
    ornare tristique ut vulputate.
  </div>

  <!-- legend -->
  <div class="legend">
    <svg width="500" height={marginHeight}>
      <g transform="translate({500 / 2 - 200} 25), scale({0.7})"
        ><Flower
          person={{ sex: "W", status: "Panthéonisée" }}
          shape={"petals"}
          hovered=""
        /><text
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
      <g transform="translate({500 / 2 - 100} 25), scale({0.7})"
        ><Flower
          person={{ sex: "M", status: "Panthéonisée" }}
          shape={"petals"}
          hovered=""
        /><text
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
      <g transform="translate({500 / 2 + 120} 25), scale({0.7})">
        <g transform="translate(-60 0)">
          <Flower
            person={{ sex: "W", status: "Partenaire" }}
            shape={"petals"}
            hovered=""
          />
        </g>

        <g transform="translate(60 0)">
          <Flower
            person={{ sex: "M", status: "Partenaire" }}
            shape={"petals"}
            hovered=""
          /></g
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
</div>

<!-- Graph -->
<div class="right">
  <div class="graph-container spiral">
    {#if data}
      <div class="inner-container">
        <svg
          {height}
          {width}
          viewBox="-{width / 2} -{height / 2} {width} {height}"
          on:mouseleave={() => {
            hovered = null;
          }}
          role="tooltip"
        >
          <!-- Year annotations -->
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

            <!-- Data points -->
            {#each data as person, index}
              <g
                transform="translate({person.spiral_x} {person.spiral_y}), scale({0.8 -
                  person.id / 150})"
                class="{person.name} flower"
                on:mouseover={() => {
                  if (hovered === null || hovered.data.id != person.id) {
                    hovered = {
                      x: person.spiral_x + width / 2,
                      y: person.spiral_y + height / 2,
                      data: person,
                    };
                  }
                }}
                on:focus={() => {
                  hovered = person;
                }}
                role="tooltip"
              >
                {#key hovered}
                  <Flower {person} shape={"petals"} {hovered} />
                {/key}
              </g>
            {/each}
          </g></svg
        >

        <!-- Tooltip -->
        {#if hovered}
          <Tooltip data={hovered} {width} />
        {/if}
      </div>
    {/if}
  </div>
</div>

<style>
  .left {
    width: 500px;
  }

  .intro-viz {
    margin-bottom: 50px;
  }

  .graph-container {
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .inner-container {
    position: relative;
  }
</style>
