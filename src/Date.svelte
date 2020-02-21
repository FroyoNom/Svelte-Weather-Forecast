<script>
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
  <p class="location">
    <i class="fas fa-map-marker-alt" />
    hello
  </p>
</div>
