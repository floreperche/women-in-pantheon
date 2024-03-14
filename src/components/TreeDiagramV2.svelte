<script>
  import { scaleLinear } from "d3";
  import Flower from "./Flower.svelte";

  export let data;
  export let width;
  export let height;
  export let marginHeight;

  // Scale for axes of flowers

  let yScale = scaleLinear()
    .domain([0, 6])
    .range([height - 50, 40]);

  let womenData = [];
  data.map((e) => {
    if (e.sex === "W") {
      womenData.push(e);
    }
  });

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
</script>

<div class="graph-container tree">
  <div class="legend">
    <svg {width} height={marginHeight}> </svg>
  </div>
  <svg {width} {height} class="viz">
    <g transform="translate(30 {height / 2}), scale(0.8)" class="flower">
      <path
        fill="#eee7ef"
        stroke="none"
        stroke-width="6"
        d="M48.1,-66.9C63,-55.4,76.2,-42.2,82.6,-26C89,-9.8,88.5,9.4,81.8,25.5C75,41.5,62,54.4,47.3,62.6C32.6,70.8,16.3,74.3,0.2,74C-15.9,73.7,-31.7,69.6,-43.7,60.6C-55.7,51.5,-63.9,37.4,-71,21.6C-78.2,5.8,-84.4,-11.8,-81.9,-28.7C-79.3,-45.6,-68,-61.8,-52.9,-73.2C-37.8,-84.6,-18.9,-91.3,-1.1,-89.7C16.6,-88.2,33.2,-78.4,48.1,-66.9Z"
        transform="scale(0.2)"
        fill-opacity="100%"
      />
    </g>
    {#each womenData as women, i}
      <path
        stroke="#eee7ef"
        stroke-width="3"
        stroke-opacity="0.6"
        stroke-linecap="round"
        fill="none"
        d={linkPathGenerator({
          x: 200,
          y: yScale(i),
          parent: { x: 30, y: height / 2 },
        })}
      />
      <g transform="translate(200 {yScale(i)})">
        <Flower person={women} shape={"petals"} hovered="" />
        <text x="50" fill="#eee7ef">{women.name}</text></g
      >
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
