<script>
  import data from "./assets/pantheon_data.json";
  import SpiralViz from "./components/SpiralViz.svelte";
  import BeeSwarmViz from "./components/BeeSwarmViz.svelte";
  import TreeDiagram from "./components/TreeDiagram.svelte";
  import logo from "./assets/logo_wildvariables.png";

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
      {#each vizOptions as option, i}
        <div
          class="selection"
          on:click={() => {
            selectedOption = option.id;
          }}
          on:keydown={() => {
            selectedOption = option.id;
          }}
          role="menuitem"
          tabindex={i}
          style="opacity : {selectedOption === option.id ? '100%' : '50%'}"
        >
          {option.value}
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
  <div class="bottom-space">
    <p>
      Source: <a
        href="https://fr.wikipedia.org/wiki/Liste_des_personnes_transf%C3%A9r%C3%A9es_au_Panth%C3%A9on_de_Paris"
        target="_blank">Wikipedia</a
      >
    </p>
    <img src={logo} alt="wild variables logo" class="logo" />
  </div>
</main>

<style>
  main {
    background-color: #120833;
    min-height: 100vh;
    height: fit-content;
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
    font-size: 30px;
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
    background-color: #120833;
    padding-bottom: 50px;
    padding-top: 20px;
    display: flex;
    justify-content: center;
    gap: 40px;
  }

  .logo {
    width: 150px;
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
    .header {
      position: -webkit-sticky;
      position: sticky;
      top: 0;
      background: linear-gradient(#120833, 95%, #12083300);
    }
    .main-content {
      gap: 0px;
    }

    .bottom-space {
      gap: 10px;
      font-size: 12px;
    }

    .logo {
      width: 100px;
    }
  }
</style>
