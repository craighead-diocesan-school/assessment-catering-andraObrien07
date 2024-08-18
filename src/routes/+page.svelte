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

  let GST = 0.15;

  // function customMenu() {
  //   foodToBuy.cart = [...foodToBuy.cart, ""];
  // }

  function removeFoodFromMenu(index) {
    menu = [...menu.slice(0, index), ...menu.slice(index, 1)];
  }
  // let selected = lists[0];
  // let lists = [];
  // let selected = lists[0];

  //AHHHHHHHHHHHHHH SHOOOOTTTTTT MEEEEEEE
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
    <h3>Menu</h3>

    <!-- <button on:click={customMenu}>+</button> -->

    <!-- <button
      on:click={() => {
        removeFoodFromMenu(index);
      }}>remove</button
    > -->

    <!-- {#each menu as menu, index}
      <button
        on:click={() => {
          removeFoodFromMenu(index);
        }}></button
      >
    {/each} -->

    {#if menu == 0}
      <p>No Food In Menu</p>
    {:else}
      <p>You have {menu.length} foods in your menu</p>
    {/if}

    {#each menu as foodToBuy}
      {foodToBuy.item}
      {foodToBuy.description}
      ${foodToBuy.price}
      + ${foodToBuy.price * GST} GST
      <img src={foodToBuy.img} alt={foodToBuy.item} />
      <!-- <button
        on:click={() => {
          removeFoodFromMenu(index);
        }}></button
      > -->
      <!-- {#each selected.foodToBuy as item}
        <input bind:value={item} />
      {/each}
      <button
        on:click={() => {
          removeFoodFromMenu(index);
        }}>remove</button
      > -->
    {/each}
    {#each menu as menu, index}
      <button
        on:click={() => {
          removeFoodFromMenu(index);
        }}>🗑️</button
      >
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
