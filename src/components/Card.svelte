<script lang="ts">
  import { createEventDispatcher } from "svelte";

  export let label: string;
  export let text: string;

  const dispatch = createEventDispatcher();

  const defaultCololr = "#cf5f42";
  const tatooineColor = "#edab00";

  $: color = label === "Tatooine" ? tatooineColor : defaultCololr;
</script>

<div class="card" on:click={() => dispatch("selectLabel", label)}>
  <div class="label" style="background-color: {color}">
    {label}
  </div>
  <span class="text">{text}</span>
</div>

<style>
  .card {
    min-width: 160px;
    height: 160px;
    margin: 8px;
    animation: sliding 30s linear infinite;
    display: flex;
    flex-direction: column;
    align-items: start;
    justify-content: space-between;
    background-color: #fff;
    padding: 8px;
    border-radius: 2px;
    cursor: pointer;
  }

  .label {
    padding: 8px;
    color: #fff;
    border-radius: 2px;
  }

  .text {
    font-size: 16px;
    text-align: left;
  }

  @keyframes sliding {
    100% {
      transform: translateX(calc(-176px * 5));
    }
  }
</style>
