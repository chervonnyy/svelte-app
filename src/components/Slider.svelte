<script lang="ts">
  import type { Character } from "../types";
  import Card from "./Card.svelte";

  export let characters: Character[] = [];
  export let selectLabel: (label: string) => void;
</script>

<section class="container">
  <div class="slider">
    {#each characters as char (char.name)}
      <Card
        label={char.home}
        text={char.name}
        on:selectLabel={(e) => selectLabel(e.detail)}
      />
    {/each}
  </div>
</section>

<style>
  .container {
    width: 100%;
    display: flex;
    justify-content: center;
    position: relative;
    top: -30px;
  }

  .slider {
    display: flex;
    height: 100%;
    width: calc(176px * 10);
  }
</style>
