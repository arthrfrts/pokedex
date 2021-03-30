<script>
  import { onMount } from "svelte";
  import ky from "ky";
  import PokeCard from "./components/PokeCard.svelte";

  let pokemons = [];

  onMount(async () => {
    let api_url = "//pokeapi.co/api/v2/pokemon";
    const data = await ky.get(api_url).json();

    pokemons = data.results;
  });
</script>

<div class="container">
  <ul class="pokemons">
    {#each pokemons as { name, url, image }, index (index)}
      <li>
        <PokeCard {name} {url} />
      </li>
    {/each}
  </ul>
</div>
