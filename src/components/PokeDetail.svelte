<script>
  import { Link } from "svelte-routing";

  import DataStats from "./DataStats.svelte";
  import Sprites from "./Sprite.svelte";
  import Type from "./Type.svelte";

  export let pokemon;

  const femaleColor = "--color-scale-pink-3";
  const maleColor = "--color-scale-blue-3";
</script>

<style>
  .pokedetails-header {
    margin-bottom: 3rem;
  }

  .pokedetails-header .pokemon-type-list {
    list-style: none;
    margin: 0;
    padding: 0;
    display: flex;
  }

  .datagroups {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-gap: var(--gutter);
  }

  @media screen and (max-width: 600px) {
    .datagroups {
      grid-template-columns: 1fr;
    }
  }
</style>

<header class="pokedetails-header">
  <h2>{pokemon.name}</h2>

  <ul class="pokemon-type-list">
    {#each pokemon.types as type (type.name)}
      <li>
        <Type name={type.name} color={type.color} />
      </li>
    {/each}
  </ul>

  <p>{pokemon.description}</p>
</header>

<Sprites sprites={pokemon.sprites} />

<div class="datagroups">
  <section class="dataset">
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

  <section class="dataset">
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

  <section class="dataset">
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

  <section class="dataset">
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
</div>

<footer>
  {#if pokemon.evolvesFrom}
    <p>
      {pokemon.name} evolves from
      <Link to="/{pokemon.evolvesFrom.url}">
        {pokemon.evolvesFrom.name}
      </Link>.
    </p>
  {:else}
    <p>{pokemon.name} does not evolve from another Pokemon.</p>
  {/if}
</footer>
