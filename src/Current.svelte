<script>
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

  const API_KEY = process.env.APIKEY2;
  let currentTemp;
  let LAT;
  let LON;
  let icon;

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
            icon = weather.weather[0].icon;

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
    align-items: center;
    margin: 0;
  }

  .temp {
    font-size: 3.5em;
    margin: 0;
    color: #fff;
  }
</style>

{#if error}
  <p>Could not find weather for this location: {location}</p>
  <button on:click={retry} type="button">Retry</button>
{:else if loading}
  <div class="loader loader--style2" title="1">
    <svg
      version="1.1"
      id="loader-1"
      xmlns="http://www.w3.org/2000/svg"
      xmlns:xlink="http://www.w3.org/1999/xlink"
      x="0px"
      y="0px"
      width="40px"
      height="40px"
      viewBox="0 0 50 50"
      style="enable-background:new 0 0 50 50;"
      xml:space="preserve">
      <path
        fill="#000"
        d="M25.251,6.461c-10.318,0-18.683,8.365-18.683,18.683h4.068c0-8.071,6.543-14.615,14.615-14.615V6.461z">
        <animateTransform
          attributeType="xml"
          attributeName="transform"
          type="rotate"
          from="0 25 25"
          to="360 25 25"
          dur="0.6s"
          repeatCount="indefinite" />
      </path>
    </svg>
  </div>
{:else}
  <div class="container">
    <h1 class="temp">{currentTemp}°</h1>
    <img src="http://openweathermap.org/img/wn/{icon}@2x.png" alt="weather" />
  </div>
{/if}
