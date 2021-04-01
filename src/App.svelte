<script>
  import { Router, Link, Route } from "svelte-routing";

  import Home from "../routes/Home.svelte";
  import Detail from "../routes/Detail.svelte";

  export let url = "";
</script>

<Router {url}>
  <header>
    <h1>
      <Link to="/">Pokédex</Link>
    </h1>
  </header>

  <main>
    <Route path="/:id" component={Detail} />
    <Route path="/" component={Home} />
  </main>
</Router>
