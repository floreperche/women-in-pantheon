<script>
  export let person;
  export let shape;
  export let hovered;
  let purplePetals = [];
  purplePetals.length = 4;
  let orangePetals = [];
  orangePetals.length = 9;

  $: flowerType = person.sex;
  $: flowerStyle = person.status === "Panthéonisée" ? "fill" : "stroke";
  $: flowerShape = shape;
  $: flowerStatus = hovered
    ? hovered.data.id === person.id
      ? "hightlighted"
      : "faded"
    : "no-highlight";
</script>

<defs>
  <linearGradient id="orangeGrad" x1="0%" x2="0%" y1="100%" y2="0%">
    <stop offset="8%" stop-color="#F34C63" />
    <stop offset="30%" stop-color="transparent" />
  </linearGradient>
  <linearGradient id="purpleGrad" x1="0%" x2="0%" y1="100%" y2="0%">
    <stop offset="20%" stop-color="#7480D2" />
    <stop offset="70%" stop-color="transparent" />
  </linearGradient>
</defs>

<!-- Orange flower -->
{#if flowerType === "W"}
  <g
    transform={flowerStatus === "hightlighted" ? "scale(1.2)" : ""}
    opacity={flowerStatus === "faded" ? "0.4" : ""}
  >
    <!-- Petals -->
    {#if flowerShape === "petals"}
      {#each orangePetals as petal, i}
        <path
          fill={flowerStyle === "fill" ? "#F34C63" : "#120833"}
          stroke={flowerStyle === "stroke" ? "url(#orangeGrad)" : "none"}
          stroke-width="2.5"
          d="M20.7,2.5C20.7,15.4,10.4,30.8,2.4,30.8C-5.7,30.8,-11.3,15.4,-11.3,2.5C-11.3,-10.3,-5.7,-20.7,2.4,-20.7C10.4,-20.7,20.7,-10.3,20.7,2.5Z"
          transform="rotate({80 * i})"
          fill-opacity={flowerStyle === "fill" ? "50%" : "100%"}
        />
      {/each}
    {/if}
    <!-- Center -->
    <path
      fill={flowerStyle === "fill" ? "#f55f74" : "transparent"}
      stroke={flowerStyle === "stroke" ? "#f55f74" : "none"}
      stroke-width="20"
      d="M48.1,-66.9C63,-55.4,76.2,-42.2,82.6,-26C89,-9.8,88.5,9.4,81.8,25.5C75,41.5,62,54.4,47.3,62.6C32.6,70.8,16.3,74.3,0.2,74C-15.9,73.7,-31.7,69.6,-43.7,60.6C-55.7,51.5,-63.9,37.4,-71,21.6C-78.2,5.8,-84.4,-11.8,-81.9,-28.7C-79.3,-45.6,-68,-61.8,-52.9,-73.2C-37.8,-84.6,-18.9,-91.3,-1.1,-89.7C16.6,-88.2,33.2,-78.4,48.1,-66.9Z"
      transform="scale(0.1)"
      fill-opacity={flowerStyle === "fill" ? "100%" : "80%"}
    /></g
  >

  <!-- Purple flower -->
{:else if flowerType === "M"}
  <g
    transform={flowerStatus === "hightlighted" ? "scale(1.2)" : ""}
    opacity={flowerStatus === "faded" ? "0.4" : ""}
  >
    <!-- Petals -->

    {#if flowerShape === "petals"}
      {#each purplePetals as petal, i}
        <path
          fill={flowerStyle === "fill" ? "#7480D2" : "#120833"}
          stroke={flowerStyle === "stroke" ? "url(#purpleGrad)" : "none"}
          stroke-width="9"
          d="M88 14C88 50 44 100 15.7 100C-12.5 100 -25 50 -25 14C-25 -22 -12.5 -44 15.7 -44C44 -44 88 -22 88 14"
          transform="rotate({90 * i})scale(0.3)"
          fill-opacity={flowerStyle === "fill" ? "50%" : "10%"}
        />
      {/each}
    {/if}
    <!-- Center -->
    <path
      fill={flowerStyle === "fill" ? "#8891d1" : "transparent"}
      stroke={flowerStyle === "stroke" ? "#8891d1" : "none"}
      stroke-width="20"
      d="M48.1,-66.9C63,-55.4,76.2,-42.2,82.6,-26C89,-9.8,88.5,9.4,81.8,25.5C75,41.5,62,54.4,47.3,62.6C32.6,70.8,16.3,74.3,0.2,74C-15.9,73.7,-31.7,69.6,-43.7,60.6C-55.7,51.5,-63.9,37.4,-71,21.6C-78.2,5.8,-84.4,-11.8,-81.9,-28.7C-79.3,-45.6,-68,-61.8,-52.9,-73.2C-37.8,-84.6,-18.9,-91.3,-1.1,-89.7C16.6,-88.2,33.2,-78.4,48.1,-66.9Z"
      transform="scale(0.1)"
      fill-opacity={flowerStyle === "fill" ? "100%" : "80%"}
    /></g
  >
{/if}
