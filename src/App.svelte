<script>
  import ky from "ky";
  import PokeCard from "./components/PokeCard.svelte";

  let pokemons = [];
  let offset = 0;
  let perPage = 24;

  $: {
    getPokemon(offset);
  }

  // Fetching Pokémons from the API.
  const getPokemon = async () => {
    let requestURL = `//pokeapi.co/api/v2/pokemon?offset=${offset}&limit=${perPage}`;

    const data = await ky.get(requestURL).json();

    pokemons = [...pokemons, ...data.results];
  };

  // Setting the next batch to load.
  const handlePagination = () => {
    offset += perPage;
  };
</script>

<div class="container">
  <ul class="pokemons">
    {#each pokemons as { name, url } (url)}
      <li>
        <PokeCard {name} {url} />
      </li>
    {/each}

    {#if pokemons.length > 0}
      <button type="button" id="load-more" on:click={handlePagination}>
        Load more Pokemons!
      </button>
    {/if}
  </ul>
</div>
