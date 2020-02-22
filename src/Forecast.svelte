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

  onMount(() => {
    location = "6174041";
    getCurrentWeather();
  });

  let location = "";
  let loading = true;
  let error = false;

  const API_KEY = config.APIKEY;

  let temps;
  let icons;
  let LAT;
  let LON;
  let dayName;
  let dayName2;
  let dayName3;
  let dayName4;
  let dayName5;
  let every;
  const days = config.DAYS;
  let week;

  const getCurrentWeather = () => {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(position => {
        LAT = position.coords.latitude;
        LON = position.coords.longitude;

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

            week = [
              weather.list[4].dt_txt,
              weather.list[12].dt_txt,
              weather.list[20].dt_txt,
              weather.list[28].dt_txt,
              weather.list[36].dt_txt
            ];

            let day1 = new Date(weather.list[4].dt_txt);
            dayName = day1.toString().split(" ")[0];

            let day2 = new Date(weather.list[12].dt_txt);
            dayName2 = day2.toString().split(" ")[0];

            let day3 = new Date(weather.list[20].dt_txt);
            dayName3 = day3.toString().split(" ")[0];

            let day4 = new Date(weather.list[28].dt_txt);
            dayName4 = day4.toString().split(" ")[0];

            let day5 = new Date(weather.list[36].dt_txt);
            dayName5 = day5.toString().split(" ")[0];

            every = [dayName, dayName2, dayName3, dayName4, dayName5];

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
  .container {
    display: flex;
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

  .days {
    display: flex;
    justify-content: space-between;
  }

  p {
    font-weight: 700;
    margin: 8px 0px;
    padding: 10px 30px;
    color: #9da4ac;
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
      <div class="days">
        {#each every as indie}
          <p>{indie}</p>
        {/each}
      </div>
    </div>
  {/if}
</div>
