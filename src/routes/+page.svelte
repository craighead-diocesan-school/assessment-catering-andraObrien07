<script>
  import Header from "$lib/Header.svelte";

  // go to the getWalls function and it will send you back the array.
  let foodToBuy = getFood();
  // let selected = carsForHire[0];

  async function getFood() {
    // hop off to the website to get the array
    let foodData = await fetch(
      "https://digitech.craighead.school.nz/api/restaurant",
    );

    // send the array to where this function was called from.
    return foodData.json();
  }
</script>

<Header />

{#await foodToBuy}
  ...waiting

  <!-- once you get the data, do this stuff -->
{:then foodToBuy}
  <!-- two diff loops to go through the two diff arrays. fast and nice -->
  <!-- {#each foodToBuy.breakfast as foodToBuy}{/each} -->
  {#each foodToBuy.breakfast as foodToBuy}
    {foodToBuy.item}
    {foodToBuy.description}
    {foodToBuy.price}
    <img src={foodToBuy.img} alt={foodToBuy.item} />
  {/each}

  {#each foodToBuy.dinner as foodToBuy}
    {foodToBuy.item}
    {foodToBuy.description}
    {foodToBuy.price}
    <img src={foodToBuy.img} alt={foodToBuy.item} />
  {/each}
{/await}
<footer>
  <p>&copy; Craighead Diocesan School 2024</p>
</footer>
