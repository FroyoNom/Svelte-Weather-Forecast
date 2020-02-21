<script>
  import { onMount } from "svelte";
  import Date from "./Date.svelte";
  import Forecast from "./Forecast.svelte";
</script>

<style>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: start;
    box-shadow: 0 10px 30px -15px;
    border-radius: 25px;
    height: 30em;
    width: 40em;
    background-color: rgb(247, 247, 247);
    margin: 2em 0 0 0;
  }
</style>

<div class="container">
  <h1>Weather</h1>
  <Date />
  <Forecast />
</div>
