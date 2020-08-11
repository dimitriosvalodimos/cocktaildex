<script>
  import Card from "./Card.svelte";
  import Searchbar from "./Searchbar.svelte";

  let cardData;

  let firstSearch = false;

  const imageHeight = 250;

  const handleCardPropagation = (event) => {
    cardData = event.detail.cocktails;
    firstSearch = true;
  };
</script>

<style>
  .app {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-auto-rows: minmax(100px, auto);
    grid-template-areas:
      "search search search"
      "cards cards cards"
      "cards cards cards"
      "spacer spacer spacer";
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
    align-content: space-between;
    justify-content: space-around;
    grid-area: cards;
    grid-template-rows: max-content;
    grid-template-columns: repeat(auto-fill, 250px);
    grid-row-gap: 1rem;
    width: 100%;
    height: 100%;
    padding: 0 5rem;
  }

  .no-result {
    display: grid;
    grid-area: cards;
    grid-column: 1 / -1;
    height: 100%;
    width: 100%;
    text-align: center;
  }
  .spacer {
    position: fixed;
    bottom: 0;
    grid-area: spacer;
    padding: 1rem;
    width: 100%;
    /* background-color: rgba(255, 255, 255, 0.9); */
    text-align: center;
  }
</style>

<div class="app">
  <div class="searchbar">
    <Searchbar on:showCards={handleCardPropagation} />
  </div>
  <div class="cards">
    {#if !firstSearch}
      <div class="no-result">
        <h1>Start by typing a cocktail name above</h1>
      </div>
    {:else if typeof cardData !== 'undefined' && cardData.length > 0}
      {#each cardData as cocktail}
        <Card {cocktail} {imageHeight} />
      {/each}
    {:else}
      <div class="no-result">
        <h1>No results matching your search where found</h1>
      </div>
    {/if}
  </div>
  <div class="spacer" />
</div>
