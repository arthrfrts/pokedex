<script>
  import ky from "ky";

  export let id;
  export let pokemon;

  /*
    We will use GitHub's Primer color system for this:
    <https://primer.style/css/support/color-system>
  */
  const TYPE_COLORS = {
    bug: "--color-scale-green-3",
    dark: "--color-scale-gray-9",
    dragon: "--color-scale-pink-9",
    electric: "--color-scale-yellow-5",
    fairy: "--color-scale-pink-1",
    fighting: "--color-scale-orange-7",
    fire: "--color-scale-red-4",
    flying: "--color-scale-orange-0",
    ghost: "--color-scale-purple-7",
    grass: "--color-scale-green-4",
    ground: "--color-scale-orange-8",
    ice: "--color-scale-gray-0",
    normal: "--color-scale-orange-1",
    poison: "--color-scale-purple-8",
    psychic: "--color-scale-pink-6",
    rock: "--color-scale-gray-7",
    steel: "--color-scale-gray-4",
    water: "--color-scale-blue-4",
  };

  const getPokemon = async (id) => {
    let loading;

    const pokeURL = `https://pokeapi.co/api/v2/pokemon/${id}/`;
    const pokeSpeciesURL = `https://pokeapi.co/api/v2/pokemon-species/${id}/`;

    try {
      loading = true;

      const pokeStats = await ky.get(pokeURL).json();

      const { name, types } = pokeStats;

      // Setting detail theme (using the Pokémon type).
      const detailTheme = TYPE_COLORS[types[types.length - 1].type.name];

      pokemon = {
        name: name.toUpperCase(),
        types: types.map((type) => ({
          name: type.type.name.toUpperCase(),
          color: TYPE_COLORS[type.type.name.toLowerCase()],
        })),
      };
    } catch (e) {
      loading = false;
    }

    console.debug(pokemon);
  };

  $: getPokemon(id);
</script>

<h1>Detail Page</h1>
<p>
  {id}
</p>
