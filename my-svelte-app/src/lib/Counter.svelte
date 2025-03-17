<script lang="ts">
  import { blur, fly, scale, slide } from "svelte/transition";

  export let initial: number = 0;  // Начальное значение с пропса
  let counter = initial

  let add = () => {
    counter++;
  }
</script>

{#if counter == 10}
<div id=cards transition:slide>
  <div id="h1">
    <h1>Hello</h1>
  </div>
</div>
{/if}

{#if counter == 20}
<div id=cards transition:scale>
  <div id="h1">
    <h1>Hello World ,_,</h1>
  </div>
</div>
{/if}

{#if counter == 30}
<div id=cards transition:blur>
  <div id="h1">
    <h1>YUPII</h1>
  </div>
</div>
{/if}

{#if counter == 50}
<div id=cards transition:slide>
  <div id="h1">
    <h1>HI ?_?</h1>
  </div>
</div>
{/if}

<div id="card">

  <h1 id="count">{counter}</h1>

  <div id="button-card">
    <button id="bun" onclick={add}>
      Кликай
    </button>
  </div>

</div>

