<script>
  import Lazy from "svelte-lazy";

  export let cocktail;

  export let imageHeight;
</script>

<style>
  .card {
    background-color: #454343;
    /* border-radius: 15px; */
  }

  .card-title {
    text-align: center;
    color: white;
  }

  .card-body {
    padding: 1rem;
    color: white;
  }

  .card-ingredient-list {
    list-style: none;
    margin-bottom: 1rem;
  }

  .card-instructions {
    margin-top: 0.5rem;
  }
</style>

<div class="card">
  <div class="card-image">
    <Lazy height={imageHeight}>
      <img src={cocktail.thumbnail} height={`${imageHeight}px`} alt="" />
    </Lazy>
    <h2 class="card-title">{cocktail.name}</h2>
  </div>
  <div class="card-body">
    <ul class="card-ingredient-list">
      {#each cocktail.ingredients as ingredient}
        <li class="card-ingredient">{ingredient}</li>
      {/each}
    </ul>
    <p class="card-instructions">{cocktail.instructions}</p>
  </div>
</div>
