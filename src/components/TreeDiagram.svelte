<script>
  import { hierarchy, cluster } from "d3";
  import Flower from "./Flower.svelte";

  export let data;
  export let width;
  export let height;
  export let marginHeight;

  //   $: console.log(root);
  let dataHierarchy = {
    name: "femmes",
    children: [
      { name: "Résistante", children: [] },
      { name: "Artiste", children: [] },
      { name: "Scientifique", children: [] },
      { name: "Femme politique", children: [] },
    ],
  };

  data
    .sort((a, b) => a.gap_death_transfered - b.gap_death_transfered)
    .map((e) => {
      if (e.sex === "W") {
        if (e.resistant === "TRUE") {
          if (e.other_art === "FALSE") {
            dataHierarchy.children[0].children.push({ name: e.name });
          }
        }
        if (e.other_art === "TRUE") {
          if (e.resistant === "TRUE") {
            dataHierarchy.children[0].children.push({ name: e.name });
          }
          dataHierarchy.children[1].children.push({ name: e.name });
        }
        if (e.science === "TRUE") {
          dataHierarchy.children[2].children.push({ name: e.name });
        }
        if (e.politic === "TRUE") {
          dataHierarchy.children[3].children.push({ name: e.name });
        }
      }
      if (e.id === data.length) {
      }
    });

  const clusterLayout = cluster().size([height + 100, width - 130]);
  const root = hierarchy(dataHierarchy, (d) => {
    return d.children;
  });
  clusterLayout(root);

  let linkPathGenerator = (d) => {
    let result =
      "M" +
      d.x +
      "," +
      d.y +
      "C" +
      d.x +
      "," +
      (d.y + d.parent.y) / 2 +
      " " +
      d.parent.x +
      "," +
      (d.y + d.parent.y) / 2 +
      " " +
      d.parent.x +
      "," +
      d.parent.y;
    return result;
  };

  // Manage Josephine Baker exception
  let bakerCoord = [];
  let bakerCalculation = (e) => {
    bakerCoord.push(e);
    if (bakerCoord.length > 1) {
      bakerCoord.push({
        x: (bakerCoord[0].x + bakerCoord[1].x) / 2,
        y: (bakerCoord[0].y + bakerCoord[1].y) / 2,
        path1: linkPathGenerator({
          x: (bakerCoord[0].x + bakerCoord[1].x) / 2,
          y: (bakerCoord[0].y + bakerCoord[1].y) / 2,
          parent: {
            x: bakerCoord[0].parent.x,
            y: bakerCoord[0].parent.y,
          },
        }),
        path2: linkPathGenerator({
          x: (bakerCoord[0].x + bakerCoord[1].x) / 2,
          y: (bakerCoord[0].y + bakerCoord[1].y) / 2,
          parent: {
            x: bakerCoord[1].parent.x,
            y: bakerCoord[1].parent.y,
          },
        }),
      });
    }
  };
</script>

<div class="graph-container tree">
  <div class="legend">
    <svg {width} height={marginHeight}> </svg>
  </div>
  <svg
    {width}
    {height}
    viewBox="-{width / 2} -{height / 2} {width} {height}"
    class="viz"
  >
    <g transform="scale(1 -1)translate(-{width / 2},-{height / 2})">
      {#each root.children as d}
        <g
          ><path
            stroke="#eee7ef"
            stroke-width="3"
            stroke-opacity="0.6"
            fill="none"
            d={linkPathGenerator(d)}
          />
          {#each d.children as e}
            <!-- {console.log(e.data.name)} -->
            {#if e.data.name === "Joséphine Baker"}
              {bakerCalculation(e)}
              {#if bakerCoord.length > 2}
                <!-- {console.log(bakerCoord[2].x)} -->
                <path
                  stroke="#eee7ef"
                  stroke-width="3"
                  stroke-opacity="0.6"
                  fill="none"
                  d={bakerCoord[2].path1}
                />
                <path
                  stroke="#eee7ef"
                  stroke-width="3"
                  stroke-opacity="0.6"
                  fill="none"
                  d={bakerCoord[2].path2}
                />
                <g
                  transform="translate({bakerCoord[2].x} {bakerCoord[2]
                    .y}), scale(0.8)"
                  class="flower"
                >
                  <Flower person={{ sex: "W", status: "Panthéonisée" }} />
                </g>
              {/if}
            {:else}
              <path
                stroke="#eee7ef"
                stroke-width="3"
                stroke-opacity="0.6"
                fill="none"
                d={linkPathGenerator(e)}
              />
              <g transform="translate({e.x} {e.y}), scale(0.8)" class="flower">
                <Flower person={{ sex: "W", status: "Panthéonisée" }} />
              </g>
              <!-- {console.log(e)} -->
            {/if}
          {/each}
          <g transform="translate({d.x} {d.y}), scale(0.8)" class="flower">
            <path
              fill="#f55f74"
              stroke="none"
              stroke-width="6"
              d="M48.1,-66.9C63,-55.4,76.2,-42.2,82.6,-26C89,-9.8,88.5,9.4,81.8,25.5C75,41.5,62,54.4,47.3,62.6C32.6,70.8,16.3,74.3,0.2,74C-15.9,73.7,-31.7,69.6,-43.7,60.6C-55.7,51.5,-63.9,37.4,-71,21.6C-78.2,5.8,-84.4,-11.8,-81.9,-28.7C-79.3,-45.6,-68,-61.8,-52.9,-73.2C-37.8,-84.6,-18.9,-91.3,-1.1,-89.7C16.6,-88.2,33.2,-78.4,48.1,-66.9Z"
              transform="scale(0.2)"
              fill-opacity="100%"
            />
          </g>
        </g>
      {/each}
    </g>
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
