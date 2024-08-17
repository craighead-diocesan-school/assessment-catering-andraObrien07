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

  let menu = [];

  function addFoodToMenu(foodToBuy) {
    menu = [...menu, foodToBuy];
  }

  let GST = 1.15;
</script>

<Header />
<div class="columns">
  <div class="column">
    {#await foodToBuy}
      ...waiting

      <!-- once you get the data, do this stuff -->
    {:then foodToBuy}
      <!-- two diff loops to go through the two diff arrays. fast and nice -->
      <!-- {#each foodToBuy.breakfast as foodToBuy}{/each} -->
      <!-- <div class="columnB"> -->
      {#each foodToBuy.breakfast as foodToBuy}
        <p>Breakfast</p>
        {foodToBuy.item}
        {foodToBuy.description}
        ${foodToBuy.price}
        <img src={foodToBuy.img} alt={foodToBuy.item} />
        <button
          on:click={() => {
            addFoodToMenu(foodToBuy);
          }}
        >
          Add To Menu</button
        >
      {/each}
      <!-- </div> -->
      {#each foodToBuy.dinner as foodToBuy}
        <p>Dinner</p>
        {foodToBuy.item}
        {foodToBuy.description}
        ${foodToBuy.price}
        <img src={foodToBuy.img} alt={foodToBuy.item} />
        <button
          on:click={() => {
            addFoodToMenu(foodToBuy);
          }}
        >
          Add To Menu</button
        >
      {/each}

      {#each foodToBuy.dessert as foodToBuy}
        <p>Dessert</p>
        {foodToBuy.item}
        {foodToBuy.description}
        ${foodToBuy.price}
        <img src={foodToBuy.img} alt={foodToBuy.item} />
        <button
          on:click={() => {
            addFoodToMenu(foodToBuy);
          }}
        >
          Add To Menu</button
        >
      {/each}
    {/await}
  </div>
  <div class="column">
    <h3>Menu</h3>
    {#if menu == 0}
      <p>No Food In Menu</p>
    {:else}
      <p>You have {menu.length} foods in your menu</p>
    {/if}

    {#each menu as foodToBuy}
      {foodToBuy.item}
      {foodToBuy.description}
      ${foodToBuy.price} + ${GST} GST
      <img src={foodToBuy.img} alt={foodToBuy.item} />
    {/each}
  </div>
</div>
<footer>
  <p>&copy; AndraC @ Craighead Diocesan School 2024</p>
</footer>

<style>
  .column {
    width: 400px; /* Fixed width of 1000px */
    /* justify-content: left; */
    max-width: 100%; /* Ensures the container doesn't overflow */
    margin: 4px auto; /* Centers the container */
  }
</style>
