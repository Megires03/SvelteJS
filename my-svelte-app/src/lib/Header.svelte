<CircularProgress style="height: 30px; width: 30px;" {progress} {closed} />
<br>
<div style="display: flex; justify-content: center">
  {#if progress == 1}
    <div class="paper-container">
      <Counter />
    </div>
  {/if}
</div>

<script lang="ts">
  import CircularProgress from '@smui/circular-progress';
  import 'svelte-material-ui/bare.css';
  import { onMount, onDestroy } from 'svelte';
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

<style>
  .closed {
    display: none;
  }
</style>