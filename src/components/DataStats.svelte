<script>
  import { tweened } from "svelte/motion";

  export let statColor;
  export let value;

  let amount = tweened(0);
  $: amount.set(value);
</script>

<progress max="100" value={$amount} style="background-color: {statColor}">
  {$amount}%
</progress>
