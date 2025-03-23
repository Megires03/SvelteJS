<CircularProgress style="height: 40px; width: 40px; margin-top: -22px;" {progress} {closed} />
<!-- <br>
<div id="title">
  <a href="https://vite.dev" target="_blank" rel="noreferrer">
    <img src={viteLogo} class="logo" alt="Vite Logo" />
  </a>
  <a href="https://svelte.dev" target="_blank" rel="noreferrer">
    <img src={svelteLogo} class="logo svelte" alt="Svelte Logo" />
  </a>
</div> -->

<div>
  {#if progress == 1}
    <div class="paper-container">
      <Counter />
    </div>
  {/if}
</div>

<script lang="ts">
  import svelteLogo from '/img/svelte.svg'
  import viteLogo from '/img/vite.svg'
  import CircularProgress from '@smui/circular-progress';
  import 'svelte-material-ui/bare.css';
  import { onDestroy } from 'svelte';
  import Counter from './Counter.svelte'

  let progress = $state(0);
  let closed = $state(false);
  let timer: NodeJS.Timer;

  onDestroy(() => {
    clearInterval(timer);
  });

  progress = 0;
  closed = false;
  clearInterval(timer);
  timer = setInterval(() => {
    progress += 0.2;

    if (progress >= 1) {
      progress = 1;
      closed = true;
      clearInterval(timer);
    }
  }, 100);

  onDestroy(() => {
    clearInterval(timer);
  });
</script>