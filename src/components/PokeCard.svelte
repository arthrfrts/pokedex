<script>
  import {Link} from 'svelte-routing';

  export let name;
  export let url;

  const splittedURL = url.split("/");
  const indexLocation = splittedURL.length - 2;

  const index = splittedURL[indexLocation];

  const imgURL = `//raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${index}.png`;
</script>

<article>
  <header>
    <h2>
      <Link to="/{index}">
        {name}
      </Link>
    </h2>
  </header>

  <img alt="" src={imgURL} role="presentation" />
</article>
