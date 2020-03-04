<script>
  import { onMount } from "svelte";

  // state
  let weather = {
    location: "",
    temp: "0",
    forecast: "n/a",
    icon: "https://placehold.it/50/50"
  };

  onMount(() => {
    location = "6174041";
    getCurrentForecast();
  });

  let location = "";
  let loading = true;
  let error = false;

  const API_KEY = process.env.APIKEY1;

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
  let week;

  const getCurrentForecast = () => {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(position => {
        LAT = position.coords.latitude;
        LON = position.coords.longitude;

        loading = true;
        fetch(
          `https://api.openweathermap.org/data/2.5/forecast?lat=${LAT}&lon=${LON}&units=metric&appid=${API_KEY}`
        )
          .then(res => res.json())
          .then(data => {
            if (data.error) {
              error = true;
              return;
            }
            weather = data;
            temps = [
              weather.list[4].main.temp.toFixed(0),
              weather.list[12].main.temp.toFixed(0),
              weather.list[20].main.temp.toFixed(0),
              weather.list[28].main.temp.toFixed(0),
              weather.list[36].main.temp.toFixed(0)
            ];
            icons = [
              weather.list[4].weather[0].icon,
              weather.list[12].weather[0].icon,
              weather.list[20].weather[0].icon,
              weather.list[28].weather[0].icon,
              weather.list[36].weather[0].icon
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

  /* .info {
    box-shadow: inset 0 0 20px 2px rgba(0, 0, 0, 0.26);
    border-radius: 25px;
  } */

  .temps {
    display: flex;
    justify-content: space-around;
    text-align: center;
  }

  .icons {
    display: flex;
    justify-content: space-around;
    text-align: center;
  }

  .days {
    display: flex;
    justify-content: space-around;
  }

  p {
    font-weight: 700;
    margin: 9px 0px;
    padding: 10px 30px;
    color: #ffffffce;
  }

  @media only screen and (max-width: 411px) {
    .info {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
    }

    .icons,
    .days,
    .temps {
      display: grid;
      justify-content: space-around;
      align-items: center;
    }
  }
</style>

<div class="container">
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
    <div class="info">
      <div class="temps">
        {#each temps as temp}
          <p>{temp}°</p>
        {/each}
      </div>
      <div class="icons">
        {#each icons as icon}
          <img
            src="http://openweathermap.org/img/wn/{icon}.png"
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
