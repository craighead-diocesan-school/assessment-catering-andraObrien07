<script>
    // exporting the information
    export let foodItem;
</script>

<!-- gets the information from the pretty print and stores it here, waiting to be called apon in the main page -->
{foodItem.item}
{foodItem.description}
${foodItem.price}
<img src={foodItem.img} alt={foodItem.item} />
