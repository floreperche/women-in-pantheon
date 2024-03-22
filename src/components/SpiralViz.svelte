<script>
  import { svg } from "d3";
  import Flower from "./Flower.svelte";
  import Tooltip from "./Tooltip.svelte";
  export let data;
  export let width;
  export let height;
  export let marginHeight;

  // Manage spiral shape
  const pointsPerRotation = 20;
  const inner = 30;
  const spiralProximity = 0.41;

  $: data
    .sort((a, b) => a.transfered_date - b.transfered_date)
    .map((e, i) => {
      let rank = i + 1;
      e["rank"] = rank;
      e["spiral_x"] =
        (inner + rank / spiralProximity) *
        Math.cos((rank / pointsPerRotation) * 2 * Math.PI);
      e["spiral_y"] =
        (inner + rank / spiralProximity) *
        Math.sin((rank / pointsPerRotation) * 2 * Math.PI);
      return e;
    });

  // Final year annotation
  let annotationCoordinates = {
    spiral_x:
      (inner + (data.length + 1) / spiralProximity) *
      Math.cos(((data.length + 1) / pointsPerRotation) * 2 * Math.PI),
    spiral_y:
      (inner + (data.length + 1) / spiralProximity) *
      Math.sin(((data.length + 1) / pointsPerRotation) * 2 * Math.PI),
  };

  // Tooltip
  let hovered = null;

  // Flower rotation
  const getTransformation = (person) => {
    if (hovered && hovered.data.id === person.id) {
      return `translate(${person.spiral_x} ${person.spiral_y}), scale(${
        0.8 - person.id / 150
      }), rotate(45)`;
    } else {
      return `translate(${person.spiral_x} ${person.spiral_y}), scale(${
        0.8 - person.id / 150
      })`;
    }
  };
</script>

<!-- Left elements -->
<div class="left">
  <h2 class="subtitle">La faible représentativité des femmes au Panthéon</h2>
  <div class="intro-viz">
    <p>
      Depuis la Révolution Française, le Panthéon à Paris est un lieu où l’on
      décide d’inhumer des personnalités faisant la fierté de la France. Ces
      “panthéonisés”, conformément à leur volonté, sont parfois accompagnés de
      leur partenaire.
    </p>
    <p>
      Sur la totalité des personnes reposant actuellement au Panthéon, les
      femmes se comptent qu’au nombre de sept. Parmi elles, cinq ont été
      panthéonisées.
    </p>
  </div>

  <!-- legend -->
  <div class="legend">
    <h3>Légende</h3>
    <div class="legend-groups">
      <div>
        <svg width="250" height={marginHeight} class="legend-elements">
          <g transform="translate(75 25), scale({0.7})"
            ><Flower
              person={{ sex: "W", status: "Panthéonisée" }}
              shape={"petals"}
              hovered=""
              filter=""
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
          <g transform="translate(175 25), scale({0.7})"
            ><Flower
              person={{ sex: "M", status: "Panthéonisée" }}
              shape={"petals"}
              hovered=""
              filter=""
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
        </svg>
      </div>
      <div>
        <svg width="250" height={marginHeight} class="legend-elements">
          <g transform="translate(125 25), scale({0.7})">
            <g transform="translate(-60 0)">
              <Flower
                person={{ sex: "W", status: "Partenaire" }}
                shape={"petals"}
                hovered=""
                filter=""
              />
            </g>

            <g transform="translate(60 0)">
              <Flower
                person={{ sex: "M", status: "Partenaire" }}
                shape={"petals"}
                hovered=""
                filter=""
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
              {#key hovered}
                <g
                  transform="translate({person.spiral_x} {person.spiral_y}), scale({0.8 -
                    person.id / 150})"
                  class="flower-group"
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
                  <Flower {person} shape={"petals"} {hovered} filter="" />
                </g>
              {/key}
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
    width: 400px;
  }

  .intro-viz {
    margin-bottom: 20px;
  }

  .legend-groups {
    display: flex;
    justify-content: center;
    width: 100%;
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

  .flower-group {
    transition: 9s;
  }

  @media (max-width: 992px) {
    .left {
      width: 100%;
      padding: 10px 20px;
    }

    h2 {
      font-size: 14px;
      letter-spacing: 0.5px;
    }

    .intro-viz {
      font-size: 12px;

      margin-bottom: 20px;
    }

    h3 {
      font-size: 12px;
    }
  }

  @media (max-width: 576px) {
    .legend-groups {
      flex-direction: column;
      justify-content: center;
    }

    .legend-groups div {
      display: flex;
      justify-content: center;
    }

    .right {
      transform: scale(0.6);
      height: fit-content;
    }
  }
</style>
