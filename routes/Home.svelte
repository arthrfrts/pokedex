<script>
  import ky from "ky";

  import PokeCard from "../src/components/PokeCard.svelte";

  let pokemons = [];
  let offset = 0;
  let perPage = 24;

  $: {
    getPokemon(offset);
  }

  const getPokemon = async () => {
    let requestURL = `https://pokeapi.co/api/v2/pokemon/?offset=${offset}&limit=${perPage}`;
    const data = await ky.get(requestURL).json();

    pokemons = [...pokemons, ...data.results];
  };

  const loadMore = () => {
    offset += perPage;
  };
</script>

<ol class="pokemon-list">
  {#each pokemons as { name, url } (url)}
    <li>
      <PokeCard {name} {url} />
    </li>
  {/each}
</ol>

<button type="submit" id="load-more" on:click={loadMore}>
  Load more Pokémon!
</button>

<style>
  .pokemon-list {
    padding-left: 0;
    margin: 0;
    list-style: none;
  }
</style>
