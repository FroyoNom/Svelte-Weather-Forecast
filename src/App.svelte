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
    box-shadow: 0 10px 30px -15px;
    border-radius: 25px;
    height: 75vh;
    width: 40em;
    background-color: #f6f5f7;
  }

  h1 {
    display: grid;
    grid-template-columns: 1fr 2.55fr;
    text-align: start;
    margin: 2em 0 0 0;
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
