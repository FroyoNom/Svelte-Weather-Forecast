<script>
  import config from "./config.js";

  const currentDate = () => {
    const monthNames = [
      "Jan",
      "Feb",
      "Mar",
      "Apr",
      "May",
      "June",
      "July",
      "Aug",
      "Sept",
      "Oct",
      "Nov",
      "Dec"
    ];
    const days = ["Sun", "Mon", "Tues", "Wed", "Thurs", "Fri", "Sat"];

    let today = new Date();
    let day = days[today.getDay()];
    let dd = String(today.getDate()).padStart(2, "0");
    let mm = monthNames[today.getMonth()];
    let yyyy = today.getFullYear();

    return (today = `${day}, ${mm} ${dd}, ${yyyy}`);
  };

  let LAT;
  let LON;
  let city;
  let country;
  let loading = true;
  let API_KEY = config.API_GEO;

  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(position => {
      LAT = position.coords.latitude;
      LON = position.coords.longitude;

      loading = true;

      fetch(
        `https://maps.googleapis.com/maps/api/geocode/json?latlng=${LAT},${LON}&key=${API_KEY}`
      )
        .then(response => response.json())
        .then(data => {
          city = data.results[0].address_components[3].short_name;
          country = data.results[0].address_components[6].short_name;
          loading = false;
        })
        .catch(err => {
          if (err.error) {
            error = true;
            return;
          }
        });
    });
  }
</script>

<style>
  .container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 70%;
    margin: 0;
    padding: 0;
    color: #9da4ac;
  }

  i {
    color: #48484a;
  }

  .lds-ellipsis {
    display: inline-block;
    position: relative;
    width: 80px;
    height: 80px;
  }
  .lds-ellipsis div {
    position: absolute;
    top: 33px;
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: #9da4ac;
    animation-timing-function: cubic-bezier(0, 1, 1, 0);
  }
  .lds-ellipsis div:nth-child(1) {
    left: 8px;
    animation: lds-ellipsis1 0.6s infinite;
  }
  .lds-ellipsis div:nth-child(2) {
    left: 8px;
    animation: lds-ellipsis2 0.6s infinite;
  }
  .lds-ellipsis div:nth-child(3) {
    left: 32px;
    animation: lds-ellipsis2 0.6s infinite;
  }
  .lds-ellipsis div:nth-child(4) {
    left: 56px;
    animation: lds-ellipsis3 0.6s infinite;
  }
  @keyframes lds-ellipsis1 {
    0% {
      transform: scale(0);
    }
    100% {
      transform: scale(1);
    }
  }
  @keyframes lds-ellipsis3 {
    0% {
      transform: scale(1);
    }
    100% {
      transform: scale(0);
    }
  }
  @keyframes lds-ellipsis2 {
    0% {
      transform: translate(0, 0);
    }
    100% {
      transform: translate(24px, 0);
    }
  }
</style>

<div class="container">
  <p class="current-date">{currentDate()}</p>
  {#if loading}
    <div class="lds-ellipsis">
      <div />
      <div />
      <div />
      <div />
    </div>
  {:else}
    <p class="location">
      <i class="fas fa-map-marker-alt" />
      {city}, {country}
    </p>
  {/if}
</div>
