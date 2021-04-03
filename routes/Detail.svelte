<script>
  import ky from "ky";

  export let id;
  export let pokemon;

  const getPokemon = async (id) => {
    const pokeURL = `https://pokeapi.co/api/v2/pokemon/${id}/`;
    const pokeSpeciesURL = `https://pokeapi.co/api/v2/pokemon-species/${id}/`;

    const pokeStats = await ky.get(pokeURL).json();
    const pokeSpecies = await ky.get(pokeSpeciesURL).json();

    console.debug(pokeStats);
    console.debug(pokeSpecies);
  };

  $: getPokemon(id);
</script>

<h1>Detail Page</h1>
<p>
  {id}
</p>
