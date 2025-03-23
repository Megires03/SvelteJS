<script lang="ts">
  import Header from './lib/Loading.svelte'
  import { blur, scale, slide } from "svelte/transition";
</script>

<main transition:slide>
  <Header />
</main>
