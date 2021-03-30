<script>
  import { onMount } from "svelte";
  import ky from "ky";

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
        <article class="pokemon">
          <header>
            <h2 class="pokemon-name">{name}</h2>
          </header>
          <img
            alt=""
            src="//raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/{index +
              1}.png"
          />
        </article>
      </li>
    {/each}
  </ul>
</div>
