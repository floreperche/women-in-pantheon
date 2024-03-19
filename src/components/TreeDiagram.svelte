<script>
  import { scaleLinear } from "d3";
  import Flower from "./Flower.svelte";
  import Tooltip from "./Tooltip.svelte";

  export let data;
  export let width;
  export let height;
  export let marginHeight;

  // Scales for flower axes
  let xScale = scaleLinear()
    .domain([0, 6])
    .range([80, width - 50]);

  let yScale = scaleLinear()
    .domain([0, 65])
    .range([height - 200, 50]);

  // Women dataset generation
  let womenData = [];
  data
    .sort((a, b) => a.transfered_date - b.transfered_date)
    .map((e) => {
      if (e.sex === "W") {
        womenData.push(e);
      }
    });

  // Line path generation
  let linkPathGenerator = (d, i) => {
    let result;
    // console.log(i);
    if (1 < i && i < 5) {
      result =
        "M" +
        d.x +
        "," +
        d.y +
        "C" +
        d.x +
        "," +
        (d.y + d.parent.y) / 2 +
        " " +
        d.parent.x +
        "," +
        (d.y + d.parent.y) / 2 +
        " " +
        d.parent.x +
        "," +
        d.parent.y;
    } else {
      result =
        "M" +
        d.x +
        "," +
        d.y +
        "C" +
        d.x +
        "," +
        (d.y + d.parent.y * 8) / 9 +
        " " +
        d.parent.x +
        "," +
        (d.y + d.parent.y + d.parent.y + d.parent.y) / 4 +
        " " +
        d.parent.x +
        "," +
        d.parent.y;
    }

    return result;
  };

  // Ticks
  let yTicks = [0, 10, 20, 30, 40, 50, 60];

  // Tooltip
  let hovered = null;

  // Filters
  const filters = [
    { key: "other_art", value: "Artiste" },
    { key: "politic", value: "Femme politique" },
    { key: "resistant", value: "Résistante" },
    { key: "science", value: "Scientifique" },
  ];
  let selectedFilter = null;
</script>

<!-- Left elements -->
<div class="left">
  <h2>Une reconnaissance plus ou moins rapide</h2>
  <div class="intro-viz">
    <p>
      En moyenne, 26 ans se sont écoulés entre le décès et la panthéonisation de
      ces femmes.
    </p>
    <p>
      Mais ce chiffre cache des disparités significatives. En 2018, un an après
      sa mort, Simone Veil est entrée au Panthéon. Trois ans plus tard, c’est à
      Joséphine Baker que l’on a rendu hommage, cette fois-ci 46 ans après sa
      mort.
    </p>
  </div>

  <!-- legend -->
  <div class="legend">
    <h3>Légende</h3>
    <div class="legend">
      <svg width="500" height={marginHeight}>
        <g transform="translate({500 / 2 - 150} 25), scale({0.7})">
          <Flower
            person={{ sex: "W", status: "Panthéonisée" }}
            shape={"petals"}
            hovered=""
            filter=""
          />
          <text
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
        <g transform="translate({500 / 2 + 120} 25), scale({0.7})">
          <Flower
            person={{ sex: "W", status: "Partenaire" }}
            shape={"petals"}
            hovered=""
            filter=""
          />
          <text
            fill="#EEE7EF"
            x="0"
            y="60"
            text-anchor="middle"
            transform="scale({1})"
            ><tspan x="0" dy="-0.5em" text-anchor="middle"
              >Femme reposant au Panthéon,</tspan
            ><tspan x="0" dy="1.2em" text-anchor="middle"
              >sans être panthéonisée</tspan
            >
          </text></g
        >
      </svg>
    </div>
  </div>

  <!-- Filter -->
  <div class="filter">
    <h3>Filtrer</h3>
    <div class="filter-nav">
      {#each filters as filter}
        <div
          on:click={() => {
            if (selectedFilter === filter) {
              selectedFilter = null;
            } else {
              selectedFilter = filter;
            }
          }}
          on:keydown={() => {
            selectedFilter = filter;
          }}
          role="menuitem"
          tabindex={1}
          style="
          background-color: {selectedFilter === filter ? '#eee7ef' : ''};
          color:{selectedFilter === filter ? '#120833' : '#eee7ef'} ; 
            border: 2px solid {selectedFilter === filter
            ? '#120833'
            : '#eee7ef'}"
        >
          {filter.value}
        </div>
      {/each}
    </div>
  </div>
</div>

<!-- Graph -->
<div class="right">
  <div class="graph-container tree">
    <div class="inner-container">
      <svg {width} {height} class="viz">
        <!-- Year gap labels -->
        <g class="tick">
          {#each yTicks as tick, i}
            <text
              fill={tick % 20 ? "none" : "#EEE7EF"}
              x="10"
              y={yScale(tick)}
              dominant-baseline="middle"
              text-anchor="middle">{tick}</text
            >
            <line
              x1="5"
              x2="15"
              y1={yScale(tick)}
              y2={yScale(tick)}
              stroke={tick % 20 ? "#EEE7EF" : "none"}
            />
          {/each}
          <text fill="#EEE7EF" x="0" y="20" dominant-baseline="middle"
            >Nombre d'années entre le décès et l'entrée au Panthéon</text
          >
        </g>

        <!-- Data points and lines -->

        <!-- White seed -->
        <g
          transform="translate({width / 2} {height - 20}), scale(0.8)"
          class="flower"
        >
          <path
            fill="#eee7ef"
            stroke="none"
            stroke-width="6"
            d="M48.1,-66.9C63,-55.4,76.2,-42.2,82.6,-26C89,-9.8,88.5,9.4,81.8,25.5C75,41.5,62,54.4,47.3,62.6C32.6,70.8,16.3,74.3,0.2,74C-15.9,73.7,-31.7,69.6,-43.7,60.6C-55.7,51.5,-63.9,37.4,-71,21.6C-78.2,5.8,-84.4,-11.8,-81.9,-28.7C-79.3,-45.6,-68,-61.8,-52.9,-73.2C-37.8,-84.6,-18.9,-91.3,-1.1,-89.7C16.6,-88.2,33.2,-78.4,48.1,-66.9Z"
            transform="scale(0.2)"
            fill-opacity="100%"
          />
        </g>
        {#each womenData as women, i}
          <!-- Lines -->
          {console.log(women)}
          <path
            stroke="#eee7ef"
            stroke-width="3"
            stroke-opacity={hovered
              ? hovered.data.id === women.id
                ? "0.8"
                : "0.1"
              : selectedFilter
                ? women[selectedFilter.key] === "TRUE"
                  ? "0.8"
                  : "0.1"
                : "0.6"}
            stroke-linecap="round"
            fill="none"
            d={linkPathGenerator(
              {
                x: xScale(i),
                y: yScale(women.gap_death_transfered),
                parent: { x: width / 2, y: height - 20 },
              },
              i
            )}
          />

          <!-- Flowers -->
          <g
            on:mouseover={() => {
              if (hovered === null || hovered.data.id != women.id) {
                hovered = {
                  x: xScale(i),
                  y: yScale(women.gap_death_transfered),
                  data: women,
                };
              }
            }}
            on:focus={() => {
              if (hovered === null || hovered.data.id != women.id) {
                hovered = {
                  x: xScale(i),
                  y: yScale(women.gap_death_transfered),
                  data: women,
                };
              }
            }}
            role="tooltip"
            on:mouseleave={() => {
              hovered = null;
            }}
            transform="translate({xScale(i)} {yScale(
              women.gap_death_transfered
            )}), scale(1.1)"
          >
            <Flower
              person={women}
              shape={"petals"}
              {hovered}
              filter={selectedFilter}
            />
          </g>
        {/each}
      </svg>

      <!-- Tooltip -->
      {#if hovered}
        <Tooltip data={hovered} {width} />
      {/if}
    </div>
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

  .filter {
    margin-top: 14px;
  }

  .filter-nav {
    display: flex;
    justify-content: space-between;
  }

  .filter-nav div {
    margin: 8px 0px;
    padding: 2px 10px;
    /* border-radius: 8px; */
    cursor: pointer;
  }

  .filter-nav div:hover {
    background-color: #eee7ef3f;
  }
</style>
