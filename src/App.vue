<template>
  <div
    id="app"
    :class="typeof weather.main != 'undefined' && weather.main.temp > 27 ? 'clear' : ''"
  >
    <main>
      <div class="about">
        <h3>myWeather</h3>
        <h6>Get to know the weather condition of different countries with just one Search</h6>
      </div>
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
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
          <div class="time">{{ timeBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "93455371bd9328252588265c34bb59fd",
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
    dateBuilder() {
      let d = new Date();
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
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
    timeBuilder() {
      let d = new Date();
      let hr = d.getHours();
      let min = d.getMinutes();
      if (min < 10) {
        min = "0" + min;
      }
      let ampm = "am";
      if (hr > 12) {
        hr -= 12;
        ampm = "pm";
      }
      // let time = t.getTime();
      return `${hr}:${min} ${ampm} `;
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Courgette&family=Fredoka+One&family=Raleway:ital,wght@1,400;1,900&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Catamaran:wght@900&display=swap");
* {
  margin: 0%;
  padding: 0%;
  box-sizing: border-box;
}
#app {
  background-image: url("./assets/clouds.jpg");
  background-position: center;
  background-size: cover;
  transition: 0.4s;
}
#app.warm {
  background-image: url("./assets/warm.jpg");
}
#app.clear {
  background-image: url("./assets/clear sky.jpg");
}
#app.cloud {
  background-image: url("./assets/clouds.jpg");
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.315),
    rgba(0, 0, 0, 0.75)
  );
}
main h3 {
  color: #fff;
  text-align: center;
  font-size: 40px;
  font-family: "Fredoka One";
}
main h6 {
  color: #fff;
  text-align: center;

  font-size: 15px;
  font-family: "Raleway", cursive;
  font-weight: 400;

  word-spacing: 0.3rem;
  margin-top: 1rem;
}
@media (min-width: 768px) {
  .search-box {
    width: 50%;
    padding-top: 3rem;
    padding-bottom: 3rem;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: auto;
  }
}
@media (max-width: 768px) {
  .search-box {
    width: 100%;
    padding-top: 2rem;
    padding-bottom: 3rem;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: auto;
  }
  main h3 {
    margin-top: 5rem;
  }
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  background-color: rgba(255, 255, 255, 8.5);
  border-radius: 0px 16px 0px 16px;
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-image: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  color: #fff;
  font-size: 37px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  font-family: "Fredoka One", cursive;
}
.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  margin-top: 1rem;
  font-style: italic;
  text-align: center;
  font-family: "Courgette", cursive;
}
.location-box .time {
  color: #fff;
  font-size: 25px;
  font-weight: 900;
  margin-top: 1rem;
  font-style: italic;
  text-align: center;
  font-family: "Catamaran", sans-serif;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 900;
  font-family: "Raleway", sans-serif;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>