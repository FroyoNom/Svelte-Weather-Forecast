<script>
  import { onMount } from "svelte";
  import config from "./config.js";

  // state
  let weather = {
    location: "",
    temp: "0",
    forecast: "n/a",
    icon: "https://placehold.it/50/50"
  };

  let location = "";
  let loading = true;
  let error = false;
  let icons;
  const API_KEY = config.APIKEY;
  const LAT = config.LAT;
  const LON = config.LON;

  onMount(() => {
    location = "6174041";
    getCurrentWeather();
  });

  let temps;
  const getCurrentWeather = () => {
    loading = true;
    fetch(
      `https://api.openweathermap.org/data/2.5/forecast?lat=${LAT}&lon=${LON}&units=metric&appid=${API_KEY}`
    )
      .then(res => res.json())
      .then(doc => {
        if (doc.error) {
          error = true;
          return;
        }
        weather = doc;
        temps = [
          weather.list[4].main.temp,
          weather.list[12].main.temp,
          weather.list[20].main.temp,
          weather.list[28].main.temp,
          weather.list[36].main.temp
        ];
        icons = [
          weather.list[4].weather[0].icon,
          weather.list[12].weather[0].icon,
          weather.list[20].weather[0].icon,
          weather.list[28].weather[0].icon,
          weather.list[36].weather[0].icon
        ];
        loading = false;
      })
      .catch(err => {
        error = true;
      });
  };

  const retry = () => {
    loading = false;
    error = false;
    location = "";
  };
</script>

<style>
  .container {
    display: flex;
  }

  .info {
    background-color: rgb(255, 255, 255);
    border-radius: 10px;
    box-shadow: 0 0 7px 1px rgba(0, 0, 0, 0.1);
  }

  .temps {
    display: flex;
    justify-content: space-between;
    text-align: center;
  }

  .icons {
    display: flex;
    justify-content: space-around;
    text-align: center;
  }

  p {
    font-weight: 700;
    margin: 8px 0px;
    padding: 10px 30px;
  }
</style>

<div class="container">
  {#if error}
    <p>Could not find weather for this location: {location}</p>
    <button on:click={retry} type="button">Retry</button>
  {:else if loading}
    <p>loading..</p>
  {:else}
    <div class="info">
      <div class="temps">
        {#each temps as temp}
          <p>{temp}°</p>
        {/each}
      </div>
      <div class="icons">
        {#each icons as icon}
          <img
            src="http://openweathermap.org/img/wn/{icon}@2x.png"
            alt="weather" />
        {/each}
      </div>
    </div>
  {/if}
</div>
