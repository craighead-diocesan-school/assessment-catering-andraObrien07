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

  let nameMenu = [];

  function addFoodToMenu(foodItem) {
    menu = [...menu, foodItem];
    foodItem.selected = true;
    foodToBuy = foodToBuy;
  }

  let GST = 0.15;

  function removeFoodFromMenu(index) {
    menu = [...menu.slice(0, index), ...menu.slice(index + 1)];
    foodItem.selected = false;
    foodToBuy = foodToBuy;
  }
</script>

<Header />
<p>Name your custom menu</p>
<input bind:value={nameMenu} />
<div class="columns">
  <div class="column">
    {#await foodToBuy}
      ...waiting
    {:then foodToBuy}
      <!-- two diff loops to go through the two diff arrays. fast and nice -->
      {#each foodToBuy.breakfast as foodItem}
        <div class:highlighted={foodItem.selected}>
          <p>Breakfast</p>
          {foodItem.item}
          {foodItem.description}
          <!-- {foodItem.selected} -->
          ${foodItem.price}
          {#if addFoodToMenu}
            <p>Already added to menu</p>
            <button
              disabled={menu.includes(foodItem)}
              on:click={() => {
                addFoodToMenu(foodItem);
              }}
            >
              Add To Menu</button
            >
          {/if}

          <img src={foodItem.img} alt={foodItem.item} />
        </div>
      {/each}

      {#each foodToBuy.dinner as foodItem}
        <div class:highlighted={foodItem.selected}>
          <p>Dinner</p>
          {foodItem.item}
          {foodItem.description}
          ${foodItem.price}
          {#if addFoodToMenu}
            <p>Already added to menu</p>
            <button
              disabled={menu.includes(foodItem)}
              on:click={() => {
                addFoodToMenu(foodItem);
              }}
            >
              Add To Menu</button
            >
          {/if}

          <img src={foodItem.img} alt={foodItem.item} />
        </div>
      {/each}

      {#each foodToBuy.dessert as foodItem}
        <div class:highlighted={foodItem.selected}>
          <p>Dessert</p>
          {foodItem.item}
          {foodItem.description}
          ${foodItem.price}

          {#if addFoodToMenu}
            <p>Already added to menu</p>
            <button
              disabled={menu.includes(foodItem)}
              on:click={() => {
                addFoodToMenu(foodItem);
              }}
            >
              Add To Menu</button
            >
          {/if}

          <img src={foodItem.img} alt={foodItem.item} />
        </div>
      {/each}
    {/await}
  </div>
  <div class="column">
    {nameMenu}
    {#if menu == 0}
      <p>No Food In Menu</p>
    {:else}
      <p>You have {menu.length} foods in your menu</p>
    {/if}

    {#each menu as foodItem, index}
      <button
        on:click={() => {
          removeFoodFromMenu(index, foodItem);
        }}>🗑️</button
      >
      {foodItem.item}
      {foodItem.description}
      ${foodItem.price}
      + ${foodItem.price * GST} GST
      <img src={foodItem.img} alt={foodItem.item} />
    {/each}
  </div>
</div>
<footer>
  <p>&copy; AndraC @ Craighead Diocesan School 2024</p>
</footer>

<style>
  .column {
    width: 300px;
    max-width: 100%;
    margin-left: 20px;
  }

  .highlighted {
    background-color: rgb(249, 162, 40);
  }
</style>
