<script>
  import Searchbar from "./Searchbar.svelte";
  import Card from "./Card.svelte";

  let cardData;

  const imageHeight = 250;

  const handleCardPropagation = (event) => {
    cardData = event.detail.cocktails;
  };
</script>

<style>
  .app {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-auto-rows: minmax(100px, auto);
    grid-template-areas: "search search search" "cards cards cards" "cards cards cards";
  }

  .searchbar {
    grid-area: search;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .cards {
    display: grid;
    justify-content: space-evenly;
    grid-area: cards;
    grid-template-rows: max-content;
    grid-template-columns: repeat(auto-fill, 250px);
    grid-row-gap: 2rem;
    width: 100%;
    height: 100%;
  }
</style>

<div class="app">
  <div class="searchbar">
    <Searchbar on:showCards={handleCardPropagation} />
  </div>
  <div class="cards">
    {#if typeof cardData !== 'undefined' && cardData.length > 0}
      {#each cardData as cocktail}
        <Card {cocktail} {imageHeight} />
      {/each}
    {:else}
      <h1>No results matching your search found</h1>
    {/if}
  </div>
</div>
