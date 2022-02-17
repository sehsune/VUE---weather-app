<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 10 ? 'warm' : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ currentDate() }}</div>
        </div>
      </div>

      <div class="weather-box" v-if="typeof weather.main != 'undefined'">
        <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
        <div class="weather">{{ weather.weather[0].main }}</div>
        <div class="pressure">{{ Math.round(weather.main.pressure) }} hPa</div>
        <div class="humidity">{{ Math.round(weather.main.humidity) }} %</div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      api_key: "926540bfc9f76eaf5d050f2df13eebf2",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    currentDate() {
      let current = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[current.getDay()];
      let date = current.getDate();
      let month = months[current.getMonth()];
      let year = current.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "montserrat", sans-serif;
}
#app {
  background-image: url("./assets/coldz-bg.jpg");
  background-size: cover;
  background-position: center;
  transition: 0.4s;
  text-align: center;
  max-width: 30rem;
  margin: 2.5rem auto;
  border-radius: 5%;
}

#app.warm {
  background-image: url("./assets/sunny-bg.jpg");
}
main {
  min-height: 90vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
  border-radius: 5%;
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 17px;
  border: none;
  appearance: none;
  border-radius: 15px;
  background: #ddd;
  transition: 0.5s;
}
.search-box .search-bar:hover {
  background: #fff;
}

.location-box .location {
  color: #fff;
  font-size: 30px;
  font-weight: 500;
  padding: 5px 5px;
  text-shadow: 3px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 400;
  padding: 5px 5px;
  text-shadow: 3px 3px rgba(0, 0, 0, 0.25);
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-weight: 900;
  font-size: 90px;
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 10px;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  margin: 20px 20px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather,
.weather-box .humidity,
.weather-box .pressure {
  color: #fff;
  font-size: 20px;
  font-weight: 600;
  padding: 5px 5px;
  text-shadow: 3px 3px rgba(0, 0, 0, 0.25);
}
</style>
