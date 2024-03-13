<script>
  import { hierarchy, cluster, scaleLinear } from "d3";
  import Flower from "./Flower.svelte";

  export let data;
  export let width;
  export let height;
  export let marginHeight;

  // Creating the hiearchy dataset
  let dataHierarchy = {
    name: "femmes",
    children: [
      { name: "Scientifique", children: [] },
      { name: "Résistante", children: [] },
      { name: "Artiste", children: [] },
      { name: "Femme politique", children: [] },
    ],
  };

  data.map((e) => {
    if (e.sex === "W") {
      if (e.science === "TRUE") {
        dataHierarchy.children[0].children.push({
          name: e.name,
          status: e.status,
          transfered_date: e.transfered_date,
        });
      }
      if (e.resistant === "TRUE") {
        dataHierarchy.children[1].children.push({
          name: e.name,
          status: e.status,
          transfered_date: e.transfered_date,
        });
      }

      if (e.other_art === "TRUE") {
        dataHierarchy.children[2].children.push({
          name: e.name,
          status: e.status,
          transfered_date: e.transfered_date,
        });
      }
      if (e.politic === "TRUE") {
        dataHierarchy.children[3].children.push({
          name: e.name,
          status: e.status,
          transfered_date: e.transfered_date,
        });
      }
    }
    if (e.id === data.length) {
    }
  });

  // Building the tree layout
  const yBreak = height / 2 - 100;
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

  // Scale for Y axes of flowers
  let yScale = scaleLinear()
    .domain([1900, 2024])
    .range([yBreak + 30, height - 30]);

  // Managing Josephine Baker exception
  let bakerCoord = [];
  let bakerCalculation = (e) => {
    bakerCoord.push(e);
    if (bakerCoord.length > 1) {
      bakerCoord.push({
        x: bakerCoord[0].x,
        y: bakerCoord[0].y,
        path1: linkPathGenerator({
          x: bakerCoord[0].x,
          y: bakerCoord[0].y,
          parent: {
            x: bakerCoord[0].parent.x,
            y: yBreak,
          },
        }),
        path2: linkPathGenerator({
          x: bakerCoord[0].x,
          y: bakerCoord[0].y,
          parent: {
            x: bakerCoord[1].parent.x,
            y: yBreak,
          },
        }),
      });
    }
  };

  // Ticks
  let yTicks = [1900, 1920, 1940, 1960, 1980, 2000, 2020];
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
    <!-- Ticks -->
    {#each yTicks as tick, i}
      <g class="tick" transform="translate(-{width / 2},-{height / 2})">
        <text
          fill={tick % 100 ? "none" : "#EEE7EF"}
          x="20"
          y={height - yScale(tick)}
          dominant-baseline="middle"
          text-anchor="middle">{tick}</text
        >
        <line
          x1="15"
          x2="25"
          y1={height - yScale(tick)}
          y2={height - yScale(tick)}
          stroke={tick % 100 ? "#EEE7EF" : "none"}
        />
      </g>
    {/each}

    <!-- Graph main elements -->
    <g transform="scale(1 -1)translate(-{width / 2 - 20}, -{height / 2})">
      {#each root.children as d}
        <g
          ><path
            stroke="#eee7ef"
            stroke-width="3"
            stroke-opacity="0.6"
            stroke-linecap="round"
            fill="none"
            d={linkPathGenerator({
              x: d.x,
              y: yBreak,
              parent: { x: d.parent.x, y: d.parent.y },
            })}
          />
          <g transform="translate({d.x} {yBreak}), scale(0.8)" class="flower">
            <path
              fill="#eee7ef"
              stroke="none"
              stroke-width="6"
              d="M48.1,-66.9C63,-55.4,76.2,-42.2,82.6,-26C89,-9.8,88.5,9.4,81.8,25.5C75,41.5,62,54.4,47.3,62.6C32.6,70.8,16.3,74.3,0.2,74C-15.9,73.7,-31.7,69.6,-43.7,60.6C-55.7,51.5,-63.9,37.4,-71,21.6C-78.2,5.8,-84.4,-11.8,-81.9,-28.7C-79.3,-45.6,-68,-61.8,-52.9,-73.2C-37.8,-84.6,-18.9,-91.3,-1.1,-89.7C16.6,-88.2,33.2,-78.4,48.1,-66.9Z"
              transform="scale(0.2)"
              fill-opacity="100%"
            />
          </g>

          {#each d.children as e}
            <!-- Manage Josephine Baker exception -->
            {#if e.data.name === "Joséphine Baker"}
              {bakerCalculation(e)}
              {#if bakerCoord.length > 2}
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
                d={linkPathGenerator({
                  x: e.x,
                  y: yScale(e.data.transfered_date),
                  parent: { x: e.parent.x, y: yBreak },
                })}
              />

              <g
                transform="translate({e.x} {yScale(
                  e.data.transfered_date
                )}), scale(0.7)"
                class="flower"
              >
                <Flower person={{ sex: "W", status: e.data.status }} />
              </g>
            {/if}
          {/each}
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
