<script>
  import Date from "./Date.svelte";
  import Forecast from "./Forecast.svelte";
  import Current from "./Current.svelte";
</script>

<style>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: start;
    -webkit-box-shadow: 0px 0px 18px -2px rgba(0, 0, 0, 0.5);
    -moz-box-shadow: 0px 0px 18px -2px rgba(0, 0, 0, 0.5);
    box-shadow: 0px 0px 18px -2px rgba(0, 0, 0, 0.5);
    border-radius: 25px;
    height: 75vh;
    width: 40em;
    background-image: linear-gradient(#339adf, #3ccbff, #75ccff);
  }

  h1 {
    display: grid;
    grid-template-columns: 1fr 2.55fr;
    text-align: start;
    margin: 2em 0 0 0;
  }

  @media only screen and (max-width: 1024px) {
    h1 {
      margin-top: 3.5em;
      grid-template-columns: 1fr;
      font-size: 3rem;
    }
  }

  @media only screen and (max-width: 768px) {
    h1 {
      margin-top: 2em;
      grid-template-columns: 1fr;
      font-size: 2rem;
    }
  }

  @media only screen and (max-width: 645px) {
    .container {
      width: 38em;
    }

    h1 {
      grid-template-columns: 1fr 2.37fr;
    }
  }

  @media only screen and (max-width: 605px) {
    .container {
      width: 36em;
    }

    h1 {
      grid-template-columns: 1fr 2.37fr;
    }
  }

  @media only screen and (max-width: 470px) {
    .container {
      width: 28em;
    }

    h1 {
      grid-template-columns: 1fr;
    }
  }

  @media only screen and (max-width: 411px) {
    .container {
      width: 22em;
      height: 80vh;
    }

    h1 {
      grid-template-columns: 1fr;
    }
  }

  @media only screen and (max-width: 370px) {
    .container {
      margin-top: 0.5em;
      width: 19em;
      height: 80vh;
    }

    h1 {
      grid-template-columns: 1fr;
      margin-top: 1em;
    }
  }
</style>

<div class="container">
  <h1>Weather</h1>
  <Date />
  <Current />
  <Forecast />
</div>
