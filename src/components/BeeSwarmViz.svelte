<script>
  import timeData from "../assets/time_periods.json";

  import Flower from "./Flower.svelte";
  import Tooltip from "./Tooltip.svelte";

  import {
    forceSimulation,
    forceX,
    forceY,
    forceCollide,
    scaleOrdinal,
  } from "d3";
  import { scaleLinear } from "d3";

  export let data;
  export let width;
  export let height;
  export let margin;
  export let marginHeight;

  $: xScale = scaleLinear()
    .domain([1791, 2024])
    .range([0 + margin.left, width - margin.right]);

  const colorRange = ["#F34C63", "#7480D2"];
  let colorScale = scaleOrdinal().domain(["W", "M"]).range(colorRange);

  const RADIUS = 8;

  const simulation = forceSimulation(data);

  $: {
    simulation
      .force(
        "x",
        forceX()
          .x((d) => xScale(d.transfered_date))
          .strength(0.9)
      )
      .force(
        "y",
        forceY()
          .y((d) => height / 2)
          .strength(0.01)
      )
      .force("collide", forceCollide().radius(RADIUS))
      .alpha(0.8)
      .alphaDecay(0.0005)
      .restart();
  }

  let nodes = [];
  simulation.on("tick", () => {
    nodes = simulation.nodes();
  });

  // Ticks
  let xTicks = [1800, 1850, 1900, 1950, 2000];

  // periods
  let periodHovered = timeData[timeData.length - 1];

  let hovered = null;
</script>

<div class="left">
  <div class="intro-viz">
    Lorem ipsum dolor sit amet consectetur. Interdum pellentesque proin duis
    accumsan rhoncus proin in eget viverra. Malesuada duis amet proin mauris
    netus fames. Auctor aliquam enim mollis placerat lorem magna cursus. Nunc
    ornare tristique ut vulputate.
  </div>
  <!-- legend -->
  <div class="legend">
    <svg width="500" height={marginHeight}>
      <g transform="translate({500 / 2 - 200} 25), scale({1.4})"
        ><Flower
          person={{ sex: "W", status: "Panthéonisée" }}
          shape={"middle"}
          hovered=""
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
          />
        </g>

        <g transform="translate(30 0)">
          <Flower
            person={{ sex: "M", status: "Partenaire" }}
            shape={"middle"}
            hovered=""
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
</div>

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
        <!-- Axes, Gridlines and labels -->
        {#each timeData as period}
          <g>
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
              y="20"
              text-anchor={periodHovered
                ? periodHovered.id > 2
                  ? "middle"
                  : "left"
                : "none"}>{period.name}</text
            >
          </g>
        {/each}
        {#each xTicks as tick, i}
          <g class="tick" transform="translate({xScale(tick)},0)">
            <text
              fill={i % 2 ? "none" : "#EEE7EF"}
              x="0"
              y={height - 10}
              text-anchor="middle">{tick}</text
            >
            <line
              x1="0"
              x2="0"
              y1={height - 30}
              y2={height - 40}
              stroke="hsla(215,15%,91%,1"
            />
          </g>
        {/each}

        <!-- Data points -->
        {#each nodes as person, index}
          <g
            transform="translate({person.x} {person.y}), scale({0.85})"
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
            <Flower {person} shape={"middle"} {hovered} />
          </g>
        {/each}
      </svg>
      {#if hovered}
        <Tooltip data={hovered} {width} {colorScale} />
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
</style>
