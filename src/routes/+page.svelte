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

  // foodToBuy = [...foodToBuy, { nameMenu: "" }];

  function addFoodToMenu(foodToBuy) {
    menu = [...menu, foodToBuy];
  }

  let GST = 0.15;

  function removeFoodFromMenu(index) {
    menu = [...menu.slice(0, index), ...menu.slice(index + 1)];
  }
</script>

<Header />
<div class="columns">
  <div class="column">
    {#await foodToBuy}
      ...waiting
    {:then foodToBuy}
      <!-- two diff loops to go through the two diff arrays. fast and nice -->
      {#each foodToBuy.breakfast as foodToBuy}
        <p>Breakfast</p>
        {foodToBuy.item}
        {foodToBuy.description}
        ${foodToBuy.price}
        <button
          on:click={() => {
            addFoodToMenu(foodToBuy);
          }}
        >
          Add To Menu</button
        >

        <img src={foodToBuy.img} alt={foodToBuy.item} />
      {/each}
      <!-- </div> -->
      {#each foodToBuy.dinner as foodToBuy}
        <p>Dinner</p>
        {foodToBuy.item}
        {foodToBuy.description}
        ${foodToBuy.price}
        <button
          on:click={() => {
            addFoodToMenu(foodToBuy);
          }}
        >
          Add To Menu</button
        >
        <img src={foodToBuy.img} alt={foodToBuy.item} />
      {/each}

      {#each foodToBuy.dessert as foodToBuy}
        <p>Dessert</p>
        {foodToBuy.item}
        {foodToBuy.description}
        ${foodToBuy.price}

        <button
          on:click={() => {
            addFoodToMenu(foodToBuy);
          }}
        >
          Add To Menu</button
        >
        <img src={foodToBuy.img} alt={foodToBuy.item} />
      {/each}
    {/await}
  </div>
  <div class="column">
    <!-- {addMenu} -->
    <h3>Menu</h3>
    <!-- {menu} -->
    <input bind:value={foodToBuy.nameMenu} />
    {#if menu == 0}
      <p>No Food In Menu</p>
    {:else}
      <p>You have {menu.length} foods in your menu</p>
    {/if}

    {#each menu as foodToBuy, index}
      <button
        on:click={() => {
          removeFoodFromMenu(index);
        }}>🗑️</button
      >
      {foodToBuy.item}
      {foodToBuy.description}
      ${foodToBuy.price}
      + ${foodToBuy.price * GST} GST
      <img src={foodToBuy.img} alt={foodToBuy.item} />
    {/each}
  </div>
</div>
<footer>
  <p>&copy; AndraC @ Craighead Diocesan School 2024</p>
</footer>

<style>
  .column {
    width: 400px;
    max-width: 100%;
    margin-left: 20px;
  }
</style>
