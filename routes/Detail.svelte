<script>
  import ky from "ky";

  export let id;
  export let pokemon;

  let loading;
  let error;

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
    const pokeURL = `https://pokeapi.co/api/v2/pokemon/${id}/`;
    const pokeSpeciesURL = `https://pokeapi.co/api/v2/pokemon-species/${id}/`;

    /*
      Flavor text is a huge array, so we will randomize which text will be shown
      for fun!
    */

    const getFlavorText = (flavorTexts) => {
      let pokeDescription = [];

      pokeDescription = flavorTexts
        .filter((flavor) => flavor.language.name === "en")
        .map((item) => item.flavor_text);

      const randomIndex = Math.floor(Math.random() * pokeDescription.length);

      return pokeDescription[randomIndex];
    };

    try {
      loading = true;

      const pokeStats = await ky.get(pokeURL).json();
      const pokeSpecies = await ky.get(pokeSpeciesURL).json();

      const { name, types, sprites, stats, height, weight } = pokeStats;
      const { flavor_text_entries, capture_rate, growth_rate } = pokeSpecies;

      // Setting detail theme (using the Pokémon type).
      const detailTheme = TYPE_COLORS[types[types.length - 1].type.name];

      // Description.
      const detailDescription = getFlavorText(flavor_text_entries);

      // Stats
      const detailStats = stats.map((stat) => {
        return {
          name: stat.stat.name.toUpperCase().replace("-", " "),
          base_stat: stat.base_stat,
        };
      });

      // Growth rate
      const growthRate = growth_rate.name.toUpperCase().replace("-", " ");

      pokemon = {
        name: name.toUpperCase(),
        types: types.map((type) => ({
          name: type.type.name.toUpperCase(),
          color: TYPE_COLORS[type.type.name.toLowerCase()],
        })),
        description: detailDescription,
        sprites,
        stats: detailStats,
        height,
        weight,
        captureRate: capture_rate,
        growthRate,
        detailTheme,
      };

      loading = false;
    } catch (e) {
      loading = false;

      const serverMessage = await e.response.text();
      error = serverMessage;
    }

    console.debug(pokemon);
  };

  $: getPokemon(id);
</script>

{#if error}
  {error}
{:else if loading}
  Loading…
{:else}
  Check your console!
{/if}
