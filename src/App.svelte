<script>
  import data from "./assets/pantheon_data.json";
  import SpiralViz from "./components/SpiralViz.svelte";
  import BeeSwarmViz from "./components/BeeSwarmViz.svelte";
  import TreeDiagram from "./components/TreeDiagram.svelte";

  // Basic dimensions
  const margin = { top: 10, right: 30, left: 30, bottom: 30 };
  let width = 600;
  let height = 500;
  let marginHeight = 100;

  // Manage navigation
  const vizOptions = [1, 2, 3];
  let selectedOption = 1;
</script>

<main>
  <!-- Header -->
  <div class="header">
    <h1>Aux grandes femmes, la Nation reconnaissante?</h1>
    <!-- Navigation -->
    <nav>
      {#each vizOptions as option}
        <div
          class="selection"
          on:click={() => {
            selectedOption = option;
          }}
          on:keydown={() => {
            selectedOption = option;
          }}
          role="menuitem"
          tabindex={1}
          style="opacity : {selectedOption === option ? '100%' : '50%'}"
        >
          Viz {option}
        </div>{/each}
    </nav>
  </div>

  <!-- Main content -->
  <div class="main-content">
    <!-- Visualisation choise -->
    {#if selectedOption === 1}
      <SpiralViz {data} {width} {height} {marginHeight} />
    {/if}
    {#if selectedOption === 2}
      <BeeSwarmViz {data} {width} {height} {margin} {marginHeight} />
    {/if}
    {#if selectedOption === 3}
      <TreeDiagram {data} {width} {height} {marginHeight} />
    {/if}
  </div>

  <!-- Footer -->
  <div class="bottom-space"></div>
</main>

<style>
  main {
    background-color: #120833;
  }
  .header {
    font-family: "Cabinet Grotesk", sans-serif;
    text-align: center;
    padding: 30px;
    padding-bottom: 10px;
    max-width: 1200px;
    margin: 0 auto;
  }

  h1 {
    font-size: 40px;
    margin-bottom: 20px;
  }

  .main-content {
    display: flex;
    justify-content: center;
    gap: 30px;
  }

  nav {
    margin-bottom: 20px;
    display: flex;
    justify-content: space-between;
    gap: 20px;
  }

  .selection {
    height: auto;
    width: 100%;
    margin: 10px 0;
    padding: 5px 15px;
    text-align: center;
    border-bottom: 3px solid;
    cursor: pointer;
    font-family: "Mochiy Pop One", sans-serif;
  }

  .bottom-space {
    height: 500px;
  }
</style>
