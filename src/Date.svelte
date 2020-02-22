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
    margin: 0 0 1rem 0;
    padding: 0;
    color: #a1a3af;
  }

  i {
    color: rgba(51, 51, 51, 0.664);
  }

  .location {
    background-color: #f9f9f9;
    border-radius: 25px;
    text-align: center;
    padding: 5px 20px;
    box-shadow: 0 0 7px 1px rgba(0, 0, 0, 0.1);
  }
</style>

<div class="container">
  <p class="current-date">{currentDate()}</p>
  {#if loading}
    <p>Loading..</p>
  {:else}
    <p class="location">
      <i class="fas fa-map-marker-alt" />
      {city}, {country}
    </p>
  {/if}
</div>
