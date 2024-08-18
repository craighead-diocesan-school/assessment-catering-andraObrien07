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

  // function removeFoodFromMenu(index) {
  //   selected.foodToBuy = [
  //     ...selected.foodToBuy.slice(0, index),
  //     ...selected.foodToBuy.slice(index + 1),
  //   ];
  // }
  // let selected = lists[0];

  //AHHHHHHHHHHHHHH SHOOOOTTTTTT MEEEEEEE

  // function removeFoodToMenu(index) {
  //   const firstPart = menu.slice(0, index);
  //   //creates a new array that includes all elements from the start of the tasks array up to (but not including) the element at index.

  //   const secondPart = menu.slice(index + 1);
  //   //creates a new array that includes all elements from the element immediately after index to the end of the tasks array.

  //   menu = [...firstPart, ...secondPart];
  //   //(...) is used to concatenate these two arrays, basically creating a new array out of the old one, but leaving out the element at index.
  // }

  // function removeFoodFromMenu(index) {
  //   selected.foodToBuy = [
  //     ...selected.foodToBuy.slice(0, index),
  //     ...selected.foodToBuy.slice(index + 1),
  //   ];
  // }
</script>

<Header />
<div class="columns">
  <div class="column">
    {#await foodToBuy}
      ...waiting
    {:then foodToBuy}
      <!-- two diff loops to go through the two diff arrays. fast and nice -->
      <!-- {#each foodToBuy.breakfast as foodToBuy}{/each} -->
      <!-- <div class="columnB"> -->
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

      <!-- {#each selected.foodToBuy as item}
        <input bind:value={item} />
      {/each}
      <button
        on:click={() => {
          removeFoodFromMenu(index);
        }}>🗑 remove</button
      > -->
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
