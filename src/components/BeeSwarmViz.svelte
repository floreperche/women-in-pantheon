<script>
  import timeData from "../assets/time_periods.json";
  import Flower from "./Flower.svelte";
  import Tooltip from "./Tooltip.svelte";

  import { forceSimulation, forceX, forceY, forceCollide } from "d3";
  import { scaleLinear } from "d3";

  export let data;
  export let width;
  export let height;
  export let margin;
  export let marginHeight;

  // Beeswarm generator
  $: xScale = scaleLinear()
    .domain([selectedFilter.startDate, 2024])
    .range([0 + margin.left, width - margin.right]);

  const RADIUS = 9;

  const simulation = forceSimulation(data);

  $: {
    simulation
      .force(
        "x",
        forceX()
          .x((d) => xScale(d.transfered_date))
          .strength(0.2)
      )
      .force(
        "y",
        forceY()
          .y((d) => height / 2)
          .strength(0.005)
      )
      .force("collide", forceCollide().radius(RADIUS))
      .alpha(0.99)
      .alphaDecay(0)
      .restart();
  }

  let nodes = [];
  simulation.on("tick", () => {
    nodes = simulation.nodes();
  });

  // Ticks
  let xTicks = [1800, 1850, 1900, 1950, 2000];

  // Animation management
  let periodHovered = timeData[timeData.length - 1];
  let hovered = null;

  // Filters
  const filters = [
    { startDate: 1791, endDate: 2024, value: "Toute la période" },
    {
      startDate: 1958,
      endDate: 2024,
      value: "Cinquième République",
    },
  ];
  let selectedFilter = filters[0];
</script>

<!-- Left elements -->
<div class="left">
  <h2>Un revirement significatif ces trente dernières années</h2>
  <div class="intro-viz">
    <p>
      C'est à la fin du XXème siècle que la physicienne et chimiste Marie
      Curie-Sklodowska devient la première femme panthéonisée. Elle le fût au
      même moment que son mari, Pierre Curie.
    </p>
    <p>
      À partir de ce moment là, 40% des personnes qui ont bénéficié de cette
      reconnaissance nationale ont été des femmes.
    </p>
  </div>

  <!-- legend -->
  <div class="legend">
    <h3>Légende</h3>
    <svg width="500" height={marginHeight}>
      <g transform="translate({500 / 2 - 200} 25), scale({1.4})"
        ><Flower
          person={{ sex: "W", status: "Panthéonisée" }}
          shape={"middle"}
          hovered=""
          filter=""
        /><text
          fill="#EEE7EF"
          x="0"
          y="60"
          text-anchor="middle"
          transform="scale({0.5})"
          ><tspan x="0" dy="-0.5em" text-anchor="middle">Femme</tspan><tspan
            x="0"
            dy="1.2em"
            text-anchor="middle">panthéonisée</tspan
          >
        </text></g
      >
      <g transform="translate({500 / 2 - 100} 25), scale({1.4})"
        ><Flower
          person={{ sex: "M", status: "Panthéonisée" }}
          shape={"middle"}
          hovered=""
          filter=""
        /><text
          fill="#EEE7EF"
          x="0"
          y="60"
          text-anchor="middle"
          transform="scale({0.5})"
          ><tspan x="0" dy="-0.5em" text-anchor="middle">Homme</tspan><tspan
            x="0"
            dy="1.2em"
            text-anchor="middle">panthéonisé</tspan
          ></text
        ></g
      >
      <g transform="translate({500 / 2 + 120} 25), scale({1.4})">
        <g transform="translate(-30 0)">
          <Flower
            person={{ sex: "W", status: "Partenaire" }}
            shape={"middle"}
            hovered=""
            filter=""
          />
        </g>

        <g transform="translate(30 0)">
          <Flower
            person={{ sex: "M", status: "Partenaire" }}
            shape={"middle"}
            hovered=""
            filter=""
          /></g
        >
        <text
          fill="#EEE7EF"
          x="0"
          y="60"
          text-anchor="middle"
          transform="scale({0.5})"
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

  <!-- Filter -->
  <div class="filter">
    <h3>Filtrer</h3>
    <div class="filter-nav">
      {#each filters as filter}<div
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
          border: 2px solid #eee7ef"
        >
          {filter.value} ({filter.startDate}-{filter.endDate})
        </div>
      {/each}
    </div>
  </div>
</div>

<!-- Graph -->
<div class="right">
  <div class="graph-container bee">
    <div
      class="inner-container"
      on:mouseleave={() => {
        periodHovered = timeData[timeData.length - 1];
      }}
      role="tooltip"
    >
      <svg {height} {width}>
        <!-- Period label -->
        <g>
          {#each timeData as period}
            <rect
              x={xScale(period.start_date)}
              y="40"
              width={xScale(period.end_date) - xScale(period.start_date)}
              height={height - 40}
              fill="#eee7ef"
              opacity={periodHovered === period ? "20%" : "0%"}
              role="tooltip"
              on:mouseover={() => {
                if (periodHovered === null || periodHovered != period) {
                  periodHovered = period;
                }
              }}
              on:focus={() => {
                if (periodHovered === null || periodHovered != period) {
                  periodHovered = period;
                }
              }}
            />
            <text
              fill={periodHovered === period ? "white" : "none"}
              x={(xScale(period.end_date) + xScale(period.start_date)) / 2}
              y="14"
              text-anchor={periodHovered
                ? periodHovered.id > 2
                  ? "middle"
                  : "left"
                : "none"}>{period.name}</text
            >
            <text
              fill={periodHovered === period ? "white" : "none"}
              x={(xScale(period.end_date) + xScale(period.start_date)) / 2}
              y="32"
              text-anchor={periodHovered
                ? periodHovered.id > 2
                  ? "middle"
                  : "left"
                : "none"}
              class="period-date">({period.start_date}-{period.end_date})</text
            >
          {/each}
        </g>

        <!-- xScale labels -->
        {#each xTicks as tick, i}
          <g class="tick" transform="translate({xScale(tick)},0)">
            <text
              fill={i % 2 ? "none" : "#EEE7EF"}
              x="0"
              y={height - 40}
              text-anchor="middle">{tick}</text
            >
            <line
              x1="0"
              x2="0"
              y1={height - 60}
              y2={height - 70}
              stroke="hsla(215,15%,91%,1"
            />
          </g>
        {/each}<text
          fill="#eee7ef"
          x={width - 320}
          y={height - 10}
          text-anchor="right">Année du transfert au Panthéon →</text
        >

        <!-- Data points -->
        {#each nodes as person, index}
          <g
            transform="translate({person.x} {person.y}), scale({0.3})"
            on:mouseover={() => {
              if (hovered === null || hovered.data.id != person.id) {
                hovered = {
                  x: person.x,
                  y: person.y,
                  data: person,
                };
              }
            }}
            on:focus={() => {
              if (hovered === null || hovered.data.id != person.id) {
                hovered = {
                  x: person.x,
                  y: person.y,
                  data: person,
                };
              }
            }}
            role="tooltip"
            on:mouseleave={() => {
              hovered = null;
            }}
          >
            <Flower {person} shape={"petals"} {hovered} filter="" />
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

  .period-date {
    font-size: 12px;
  }

  .filter {
    margin-top: 14px;
  }

  .filter-nav {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .filter-nav div {
    margin: 8px 0px;
    padding: 2px 10px;
    cursor: pointer;
    width: 70%;
  }

  .filter-nav div:hover {
    background-color: #eee7ef3f;
  }
</style>
