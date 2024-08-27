<script>
  import Header from "$lib/Header.svelte";
  import Card from "$lib/Card.svelte";
  // go to the getfood function and it will send you back the array.
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
  //array storing the foods that have been added to the menu
  let menu = [];

  //holds the custom name that the user inputs
  let nameMenu = [];

  //adds a specific food to the menu array
  function addFoodToMenu(foodItem) {
    menu = [...menu, foodItem];
    //sets the selcted to true so that the highlight can run
    foodItem.selected = true;
    foodToBuy = foodToBuy;
  }

  //sets the gst to 15% which is ready to be plussed to each food
  const GST = 0.15;

  //removes food from the menu (dependent on its index)
  function removeFoodFromMenu(index, foodItem) {
    menu = [...menu.slice(0, index), ...menu.slice(index + 1)];
    //sets the selected to false to remove the highlight from the food when removed from menu
    foodItem.selected = false;
    foodToBuy = foodToBuy;
  }
</script>

<Header />
<p>Name your custom menu</p>

<!-- user can input their custom menu name -->
<input bind:value={nameMenu} />
<div class="columns">
  <div class="column">
    <!-- waiting for the food gto be fetched from the array -->
    {#await foodToBuy}
      ...waiting
    {:then foodToBuy}
      <!-- three diff loops to go through the three diff arrays. breakfast, dinner, and dessert -->

      <!-- looping through the breakfast array and displaying each foodToBuy from that array -->
      {#each foodToBuy.breakfast as foodItem}
        <!-- highlight the fooitem if the add to menu button has been clicked which changes the selected to true -->
        <div class:highlighted={foodItem.selected}>
          <p>Breakfast</p>
          <!-- calls the food information fropm the card and displays it on the page  -->
          <Card {foodItem} />

          {#if addFoodToMenu}
            <!-- disable if food is already in the menu array  -->
            <button
              disabled={menu.includes(foodItem)}
              on:click={() => {
                addFoodToMenu(foodItem);
              }}
            >
              Add To Menu</button
            >
            <!-- triggers the addFoodToMenu function when the add to menu button it clicked -->
          {/if}
        </div>
      {/each}

      <!-- looping through the dinner array and displaying each foodToBuy from that array -->
      {#each foodToBuy.dinner as foodItem}
        <!-- highlight the fooitem if the add to menu button has been clicked which changes the selected to true -->
        <div class:highlighted={foodItem.selected}>
          <p>Dinner</p>
          <Card {foodItem} />
          <!-- calls the food information fropm the card and displays it on the page  -->
          {#if addFoodToMenu}
            <p>Already added to menu</p>
            <!-- disable if food is already in the menu array  -->
            <button
              disabled={menu.includes(foodItem)}
              on:click={() => {
                addFoodToMenu(foodItem);
              }}
            >
              Add To Menu</button
            >
            <!-- triggers the addFoodToMenu function when the add to menu button it clicked -->
          {/if}
        </div>
      {/each}

      <!-- looping through the dessert array and displaying each foodToBuy from that array -->
      {#each foodToBuy.dessert as foodItem}
        <div class:highlighted={foodItem.selected}>
          <p>Dessert</p>
          <Card {foodItem} />
          <!-- calls the food information fropm the card and displays it on the page  -->
          {#if addFoodToMenu}
            <p>Already added to menu</p>
            <!-- disable if food is already in the menu array  -->
            <button
              disabled={menu.includes(foodItem)}
              on:click={() => {
                addFoodToMenu(foodItem);
              }}
            >
              Add To Menu</button
            >
            <!-- triggers the addFoodToMenu function when the add to menu button it clicked -->
          {/if}
        </div>
      {/each}
    {/await}
  </div>
  <div class="column">
    {nameMenu}
    <!-- if the menu has 0 foods in it this message will show -->
    {#if menu == 0}
      <p>No Food In Menu</p>
      <!-- if the menu has anything other than 0 items it will show the messgae below -->
    {:else}
      <p>You have {menu.length} foods in your menu</p>
    {/if}

    {#each menu as foodItem, index}
      <!-- triggers the removeFoodFromMenu function and makes a button to trigger that function -->
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
  /* css */
  .column {
    width: 300px;
    max-width: 100%;
    margin-left: 20px;
  }
  /* css for selected food */
  .highlighted {
    background-color: rgb(249, 162, 40);
  }
</style>
