<script>
  import { Link } from "svelte-routing";

  import DataStats from "./DataStats.svelte";
  import Sprites from "./Sprite.svelte";

  export let pokemon;

  const femaleColor = "--color-scale-pink-3";
  const maleColor = "--color-scale-blue-3";
</script>

<header>
  <h2>{pokemon.name}</h2>
  <p>{pokemon.description}</p>

  <ul>
    {#each pokemon.types as type (type.name)}
      <li style="border-color: var({type.color}); color: var({type.color});">
        {type.name}
      </li>
    {/each}
  </ul>
</header>

<figure>
  <Sprites sprites={pokemon.sprites} />
</figure>

<section>
  <h3>Profile</h3>
  <dl>
    <dt>Species:</dt>
    <dd>{pokemon.species}</dd>
    <dt>Height:</dt>
    <dd>
      {pokemon.height / 10} m ({Math.round(
        (pokemon.height * 0.328084 + 0.00001) * 100
      ) / 100}
      lb)
    </dd>
    <dt>Weight:</dt>
    <dd>
      {pokemon.weight / 10} kg ({Math.round(
        (pokemon.weight * 0.220462 + 0.00001) * 100
      ) / 100}
      lb)
    </dd>
    <dt>Abilities:</dt>
    <dd>{pokemon.abilities}</dd>
  </dl>
</section>

<section>
  <h3>Training Stats</h3>
  <dl>
    <dt>EV Yield:</dt>
    <dd>{pokemon.evs}</dd>
    <dt>Capture Rate:</dt>
    <dd>{pokemon.captureRate}</dd>
    <dt>Growth Rate:</dt>
    <dd>{pokemon.growthRate}</dd>
  </dl>
</section>

<section>
  <h3>Breeding Stats</h3>
  <dl>
    <dt>Gender Ratio (%F / %M):</dt>
    <dd>
      <DataStats value={pokemon.genderRatio.female} statColor={femaleColor} />
      <DataStats value={pokemon.genderRatio.male} statColor={maleColor} />
    </dd>
    <dt>Egg Groups:</dt>
    <dd>
      {#each pokemon.eggGroups as egg}{egg}{/each}
    </dd>
  </dl>
</section>

<section>
  <h2>Base Stats</h2>
  <dl>
    {#each pokemon.stats as stat}
      <div>
        <dt>{stat.name}</dt>
        <dd>
          <DataStats value={stat.base_stat} statColor={pokemon.detailTheme} />
        </dd>
      </div>
    {/each}
  </dl>
</section>

<section>
  <h3>Base Stats</h3>
  <dl>
    {#each pokemon.stats as stat}
      <dt>{stat.name}</dt>
      <dd>
        <DataStats value={stat.base_stat} statColor={pokemon.detailTheme} />
      </dd>
    {/each}
  </dl>
</section>

<footer>
  {#if pokemon.evolvesFrom}
    <p>
      {pokemon.name} evolves from
      <Link to="/{pokemon.evolvesFrom.url}">
        {pokemon.evolvesFrom.name}
      </Link>
    </p>
  {:else}
    <p>{pokemon.name} does not evolve from another Pokemon</p>
  {/if}
</footer>
