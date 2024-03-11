<script>
  import data from "./assets/pantheon_data.json";

  import SpiralViz from "./components/SpiralViz.svelte";
  import BeeSwarmViz from "./components/BeeSwarmViz.svelte";

  const margin = { top: 10, right: 30, left: 30, bottom: 30 };

  let width = 600;
  let height = 500;
  let marginHeight = 150;

  // Bee-swarm chart

  // console.log(simulation.nodes());
</script>

<main>
  <div class="intro">
    <h1>Aux grandes femmes, la Nation reconnaissante?</h1>
    <p>Dataviz</p>
  </div>

  <SpiralViz {data} {width} {height} {marginHeight} />

  <BeeSwarmViz {data} {width} {height} {margin} {marginHeight} />
  <div class="bottom-space"></div>
</main>

<style>
  main {
    background-color: #120833;
  }
  .intro {
    font-family: "Cabinet Grotesk", sans-serif;
    text-align: center;
  }

  .bottom-space {
    height: 100px;
  }
</style>
