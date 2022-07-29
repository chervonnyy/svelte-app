<script lang="ts">
  import { onMount } from "svelte";
  import axios from "axios";
  import Slider from "./components/Slider.svelte";
  import type { Character } from "./types";

  let characters: Character[] = [];
  let selectedLabel: string | undefined;

  $: citizens = characters.filter(({ home }) => home === selectedLabel);

  onMount(async () => {
    const { data } = await axios("https://swapi.dev/api/people");
    characters = await Promise.all(
      data.results.map(async (char) => {
        const { data } = await axios(char.homeworld);
        return {
          name: char.name,
          home: data.name,
        };
      })
    );

    console.log(characters);
  });
</script>

<main>
  <div class="header">
    <span class="subtitle">This is</span>
    <span class="title">Svelte<br />app with ts</span>
  </div>
  {#if characters.length}
    <Slider {characters} selectLabel={(label) => (selectedLabel = label)} />
  {/if}
  <div class="label">
    {#if selectedLabel}
      <span class="selected">{selectedLabel}</span>
      <ul>
        {#each citizens as char (char.name)}
          <li>{char.name}</li>
        {/each}
      </ul>
    {:else if characters.length}
      <span class="label label">Pleace select a card</span>
    {/if}
  </div>
</main>

<style>
  main {
    text-align: center;
    max-width: 240px;
    margin: 0 auto;
  }

  .header {
    padding: 60px 30px 0;
    text-transform: uppercase;
    font-weight: 900;
  }

  .subtitle {
    font-size: 32px;
  }

  .title {
    font-size: 128px;
    line-height: 100px;
  }

  .label {
    font-size: 16px;
  }

  .label .selected {
    font-size: 64px;
    font-weight: 900;
  }

  ul {
    display: flex;
    justify-content: space-around;
  }

  li {
    list-style: none;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
