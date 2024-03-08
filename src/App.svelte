<script>
  import data from "./assets/pantheon_data.json";
  const margin = { top: 10, right: 100, left: 100, bottom: 30 };

  let width = 800;
  let height = 600;
</script>

<main>
  <div class="intro">
    <h1>Aux grandes femmes, la Nation reconnaissante?</h1>
    <p>Dataviz</p>
  </div>
  <div class="graph-container">
    <svg {width} {height}></svg>
  </div>
</main>

<style>
  .intro {
    font-family: "Cabinet Grotesk", sans-serif;
    text-align: center;
  }

  .graph-container {
    margin: 0 auto;
    display: flex;
    justify-content: center;
  }

  svg {
    background-color: rgb(243, 245, 222);
  }
</style>
