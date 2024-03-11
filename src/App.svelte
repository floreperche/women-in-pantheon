<script>
  import data from "./assets/pantheon_data.json";
  import { forceSimulation, forceX, forceY, forceCollide } from "d3";
  import { scaleLinear, scaleBand } from "d3";
  import SpiralViz from "./components/SpiralViz.svelte";
  import Flower from "./components/Flower.svelte";

  // const margin = { top: 10, right: 100, left: 100, bottom: 30 };

  let width = 600;
  let height = 500;
  let marginHeight = 150;

  // Bee-swarm chart

  $: xScale = scaleLinear().domain([1791, 2024]).range([0, width]);

  $: yScale = scaleBand().domain(["M", "W"]).range([height, 100]);

  const RADIUS = 7;

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
          .strength(0.8)
      )
      .force(
        "y",
        forceY()
          .y((d) => yScale(d.sex))
          .strength(0.2)
      )
      .force("collide", forceCollide().radius(RADIUS))
      .alpha(0.3)
      .alphaDecay(0.0005)
      .restart();
  }

  // console.log(simulation.nodes());
</script>

<main>
  <div class="intro">
    <h1>Aux grandes femmes, la Nation reconnaissante?</h1>
    <p>Dataviz</p>
  </div>

  <SpiralViz {data} {width} {height} {marginHeight} />

  <div class="graph-container bee">
    <svg {height} {width}>
      {#each nodes as person}
        <circle
          cx={person.x}
          cy={person.y}
          r="5"
          fill={person.sex === "W" ? "#F34C63" : "#7480D2"}
          stroke="#120833"
          class={person.name}
        />
      {/each}
    </svg>
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
