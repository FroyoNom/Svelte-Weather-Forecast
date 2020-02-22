<script>
  import config from "./config.js";
  import { onMount } from "svelte";

  let weather = {
    location: "",
    temp: "0",
    forecast: "n/a",
    icon: "https://placehold.it/50/50"
  };

  onMount(() => {
    location = "6174041";
    getCurrentWeather();
  });

  const API_KEY = "9c69ed0ea5b22a10bd2d38877de28506";
  let currentTemp;
  let LAT;
  let LON;

  let location = "";
  let loading = true;
  let error = false;

  const getCurrentWeather = () => {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(position => {
        LAT = position.coords.latitude;
        LON = position.coords.longitude;

        loading = true;
        fetch(
          `https://api.openweathermap.org/data/2.5/weather?lat=${LAT}&lon=${LON}&units=metric&appid=${API_KEY}`
        )
          .then(res => res.json())
          .then(data => {
            if (data.error) {
              error = true;
              return;
            }

            weather = data;
            currentTemp = weather.main.temp.toFixed(0);

            loading = false;
          })
          .catch(err => {
            error = true;
          });
      });
    }
  };

  const retry = () => {
    loading = false;
    error = false;
    location = "";
  };
</script>

<style>
  .temp {
    margin: 1em 0;
  }
</style>

{#if error}
  <p>Could not find weather for this location: {location}</p>
  <button on:click={retry} type="button">Retry</button>
{:else if loading}
  <p>Loading..</p>
{:else}
  <h1 class="temp">{currentTemp}°</h1>
{/if}
