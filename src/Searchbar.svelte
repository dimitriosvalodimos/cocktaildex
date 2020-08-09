<script>
  import { createEventDispatcher } from "svelte";
  import { element } from "svelte/internal";
  const dispatch = createEventDispatcher();

  const BASE_URL = "https://www.thecocktaildb.com/api/json/v1/1/search.php?s=";

  let searchValue = "";

  const propagateCocktailsUp = (cocktails) => {
    dispatch("showCards", { cocktails });
  };

  const getName = (obj) => obj.strDrink;
  const getThumbnail = (obj) => obj.strDrinkThumb;
  const getInstructions = (obj) => obj.strInstructions;
  const getIngredients = (obj) => {
    const result = [];
    for (let i = 1; i <= 15; i++) {
      result.push(obj[`strIngredient${i}`]);
    }
    return result.filter((ingredient) => ingredient !== null);
  };
  const getMeasurements = (obj) => {
    const result = [];
    for (let i = 1; i <= 15; i++) {
      result.push(obj[`strMeasure${i}`]);
    }
    return result.filter((measure) => measure !== null);
  };
  const combineIngredientsAndMeasurements = (obj) => {
    const result = [];
    const ingredients = getIngredients(obj);
    const measurements = getMeasurements(obj);

    measurements.forEach((measurement, idx) => {
      result.push(`${measurement} ${ingredients[idx]}`);
    });
    return result;
  };

  const extractComponents = (items) => {
    const result = [];
    items.drinks.forEach((cocktailObj) => {
      result.push({
        name: getName(cocktailObj),
        thumbnail: getThumbnail(cocktailObj),
        instructions: getInstructions(cocktailObj),
        ingredients: combineIngredientsAndMeasurements(cocktailObj),
      });
    });
    propagateCocktailsUp(result);
  };

  const handleSearch = async (e) => {
    if (e.key === "Enter") {
      const FULL_URL = `${BASE_URL}${searchValue}`;
      const searchResponse = await fetch(FULL_URL).then((res) => res.json());
      extractComponents(searchResponse);
    }
  };
</script>

<style>
  .search-container {
    text-align: center;
    margin: 3rem;
    width: 100%;
  }
  .search-title {
    font-size: 3rem;
  }

  .searchbar {
    border: none;
    margin-top: 2rem;
    width: max(50%, 250px);
    text-align: left;
    padding: 1rem 1rem;
    filter: drop-shadow(0px 5px 5px rgba(0, 0, 0, 0.25));
  }
</style>

<div class="search-container">
  <h1 class="search-title">CocktailDex</h1>
  <input
    class="searchbar"
    type="text"
    placeholder="🔍 Search for a cocktail..."
    bind:value={searchValue}
    on:keypress={handleSearch} />
</div>
