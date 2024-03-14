<script>
  import data from "./assets/pantheon_data.json";
  import SpiralViz from "./components/SpiralViz.svelte";
  import BeeSwarmViz from "./components/BeeSwarmViz.svelte";
  import TreeDiagram from "./components/TreeDiagram.svelte";

  const margin = { top: 10, right: 30, left: 30, bottom: 30 };

  let width = 600;
  let height = 500;
  let marginHeight = 100;

  const VizOptions = [1, 2, 3];
  let selectedOption = 1;
</script>

<main>
  <div class="intro">
    <h1>Aux grandes femmes, la Nation reconnaissante?</h1>
  </div>

  <div class="main-content">
    <div class="left">
      <nav>
        {#each VizOptions as option}
          <div class="nav-element">
            <div
              class="selection"
              on:click={() => {
                selectedOption = option;
              }}
              on:keydown={() => {
                selectedOption = option;
              }}
              role="menuitem"
              tabindex={option}
            >
              Option {option}
            </div>
            {#if selectedOption === option}
              <div class="intro-text">
                Lorem ipsum dolor sit amet consectetur. Interdum pellentesque
                proin duis accumsan rhoncus proin in eget viverra. Malesuada
                duis amet proin mauris netus fames. Auctor aliquam enim mollis
                placerat lorem magna cursus. Nunc ornare tristique ut vulputate.
              </div>
            {/if}
          </div>
        {/each}
      </nav>
    </div>

    <div class="right">
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
  </div>

  <div class="bottom-space"></div>
</main>

<style>
  main {
    background-color: #120833;
  }
  .intro {
    font-family: "Cabinet Grotesk", sans-serif;
    text-align: center;
    padding: 30px;
  }

  h1 {
    font-size: 40px;
  }

  .main-content {
    display: flex;
    justify-content: center;
    gap: 30px;
  }

  .left {
    width: 500px;
  }

  .nav-element {
    margin-bottom: 20px;
  }

  .selection {
    height: auto;
    width: auto;
    margin: 10px 0;
    padding: 5px 10px;
    text-align: center;
    background-color: #eee7ef;
    color: #120833;
    cursor: pointer;
    border-radius: 3px;
  }

  .bottom-space {
    height: 500px;
  }
</style>
