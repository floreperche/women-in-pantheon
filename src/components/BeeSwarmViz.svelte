<script>
  import timeData from "../assets/time_periods.json";

  import Flower from "./Flower.svelte";
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

  let simulation = forceSimulation(data);
  let nodes = [];

  simulation.on("tick", () => {
    nodes = simulation.nodes();
  });

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
      .alpha(0.2)
      .alphaDecay(0.005)
      .restart();
  }

  // Ticks
  let xTicks = [1800, 1850, 1900, 1950, 2000];
</script>

<div class="graph-container bee">
  <!-- Legend -->
  <div class="legend">
    <svg {width} height={marginHeight}>
      <g
        transform="translate({width / 2 - 200} {marginHeight / 2 -
          10}), scale({0.7})"
      >
        <path
          fill="#f55f74"
          stroke="none"
          stroke-width="6"
          d="M48.1,-66.9C63,-55.4,76.2,-42.2,82.6,-26C89,-9.8,88.5,9.4,81.8,25.5C75,41.5,62,54.4,47.3,62.6C32.6,70.8,16.3,74.3,0.2,74C-15.9,73.7,-31.7,69.6,-43.7,60.6C-55.7,51.5,-63.9,37.4,-71,21.6C-78.2,5.8,-84.4,-11.8,-81.9,-28.7C-79.3,-45.6,-68,-61.8,-52.9,-73.2C-37.8,-84.6,-18.9,-91.3,-1.1,-89.7C16.6,-88.2,33.2,-78.4,48.1,-66.9Z"
          transform="scale(0.2)"
          fill-opacity="100%"
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
      <g
        transform="translate({width / 2 - 100} {marginHeight / 2 -
          10}), scale({0.7})"
      >
        <path
          fill="#8891d1"
          stroke="none"
          stroke-width="6"
          d="M48.1,-66.9C63,-55.4,76.2,-42.2,82.6,-26C89,-9.8,88.5,9.4,81.8,25.5C75,41.5,62,54.4,47.3,62.6C32.6,70.8,16.3,74.3,0.2,74C-15.9,73.7,-31.7,69.6,-43.7,60.6C-55.7,51.5,-63.9,37.4,-71,21.6C-78.2,5.8,-84.4,-11.8,-81.9,-28.7C-79.3,-45.6,-68,-61.8,-52.9,-73.2C-37.8,-84.6,-18.9,-91.3,-1.1,-89.7C16.6,-88.2,33.2,-78.4,48.1,-66.9Z"
          transform="scale(0.2)"
          fill-opacity="100%"
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
      <g
        transform="translate({width / 2 + 150} {marginHeight / 2 -
          10}), scale({0.7})"
      >
        <g transform="translate(-60 0)">
          <path
            fill="none"
            stroke="#f55f74"
            stroke-width="6"
            d="M48.1,-66.9C63,-55.4,76.2,-42.2,82.6,-26C89,-9.8,88.5,9.4,81.8,25.5C75,41.5,62,54.4,47.3,62.6C32.6,70.8,16.3,74.3,0.2,74C-15.9,73.7,-31.7,69.6,-43.7,60.6C-55.7,51.5,-63.9,37.4,-71,21.6C-78.2,5.8,-84.4,-11.8,-81.9,-28.7C-79.3,-45.6,-68,-61.8,-52.9,-73.2C-37.8,-84.6,-18.9,-91.3,-1.1,-89.7C16.6,-88.2,33.2,-78.4,48.1,-66.9Z"
            transform="scale(0.2)"
            fill-opacity="80%"
          />
        </g>

        <g transform="translate(60 0)">
          <path
            fill="none"
            stroke="#8891d1"
            stroke-width="6"
            d="M48.1,-66.9C63,-55.4,76.2,-42.2,82.6,-26C89,-9.8,88.5,9.4,81.8,25.5C75,41.5,62,54.4,47.3,62.6C32.6,70.8,16.3,74.3,0.2,74C-15.9,73.7,-31.7,69.6,-43.7,60.6C-55.7,51.5,-63.9,37.4,-71,21.6C-78.2,5.8,-84.4,-11.8,-81.9,-28.7C-79.3,-45.6,-68,-61.8,-52.9,-73.2C-37.8,-84.6,-18.9,-91.3,-1.1,-89.7C16.6,-88.2,33.2,-78.4,48.1,-66.9Z"
            transform="scale(0.2)"
            fill-opacity="80%"
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

  <svg {height} {width}>
    <!-- Axes, Gridlines and labels -->
    {#each timeData as period}
      <rect
        x={xScale(period.start_date)}
        y="0"
        width={xScale(period.end_date) - xScale(period.start_date)}
        {height}
        fill="#eee7ef"
        opacity={period.id % 2 ? "20%" : "8%"}
      />
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
    {#each nodes as person}
      <g transform="translate({person.x} {person.y}), scale({1})"
        ><path
          fill={person.status === "Panthéonisée"
            ? colorScale(person.sex)
            : "none"}
          stroke={person.status != "Panthéonisée"
            ? colorScale(person.sex)
            : "#120833"}
          stroke-width="10"
          d="M48.1,-66.9C63,-55.4,76.2,-42.2,82.6,-26C89,-9.8,88.5,9.4,81.8,25.5C75,41.5,62,54.4,47.3,62.6C32.6,70.8,16.3,74.3,0.2,74C-15.9,73.7,-31.7,69.6,-43.7,60.6C-55.7,51.5,-63.9,37.4,-71,21.6C-78.2,5.8,-84.4,-11.8,-81.9,-28.7C-79.3,-45.6,-68,-61.8,-52.9,-73.2C-37.8,-84.6,-18.9,-91.3,-1.1,-89.7C16.6,-88.2,33.2,-78.4,48.1,-66.9Z"
          transform="scale(0.1)"
          fill-opacity={person.status === "Panthéonisée" ? "100%" : "80%"}
        />
      </g>
    {/each}
  </svg>
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
