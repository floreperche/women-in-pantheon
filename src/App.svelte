<script>
  import data from "./assets/pantheon_data.json";
  import SpiralViz from "./components/SpiralViz.svelte";
  import BeeSwarmViz from "./components/BeeSwarmViz.svelte";
  import TreeDiagram from "./components/TreeDiagram.svelte";

  // Basic dimensions
  const margin = { top: 10, right: 30, left: 30, bottom: 30 };
  let width = 500;
  let height = 500;
  let marginHeight = 100;

  // Manage navigation
  const vizOptions = [
    { id: 1, value: "Vue d'ensemble" },
    { id: 2, value: "Evolution" },
    { id: 3, value: "En savoir plus" },
  ];
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
            selectedOption = option.id;
          }}
          on:keydown={() => {
            selectedOption = option.id;
          }}
          role="menuitem"
          tabindex={1}
          style="opacity : {selectedOption === option.id ? '100%' : '50%'}"
        >
          {option.value}
        </div>{/each}
    </nav>
  </div>

  <div style="width:320px; background-color : blue; height : 20px;"></div>
  <div style="width:576px; background-color : green; height : 20px;"></div>

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
    padding: 20px;
    padding-bottom: 10px;
    max-width: 1000px;
    margin: 0 auto;
  }

  h1 {
    font-size: 36px;
    margin-bottom: 20px;
  }

  nav {
    margin-bottom: 20px;

    display: flex;
    justify-content: space-between;
    align-items: end;
    gap: 20px;
  }

  .selection {
    width: 100%;
    padding: 5px 15px;
    text-align: center;
    border-bottom: 3px solid;
    cursor: pointer;
    font-family: "Mochiy Pop One", sans-serif;
  }

  .main-content {
    display: flex;
    justify-content: center;
    gap: 30px;
  }

  .bottom-space {
    height: 10px;
  }

  @media (max-width: 992px) {
    .header {
      padding: 10px;
    }
    h1 {
      font-size: 18px;
    }
    .selection {
      font-size: 12px;
      padding: 5px 4px;
    }

    .main-content {
      flex-direction: column;
    }
  }

  @media (max-width: 576px) {
    .main-content {
      gap: 10px;
    }
  }
</style>
