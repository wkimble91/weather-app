<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 70 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather" />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°f</div>
          <div class="weather-range-box">
            <div class="temp-range">{{ Math.round(weather.main.temp_max) }}°f High,
              {{ Math.round(weather.main.temp_min) }}°f Low</div>
          </div>
          <div class="weather-status">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      api_key: 'c6b5cc190ef380ef6c677a6118f9c536',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key=="Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=imperial&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults(results) {
      this.weather=results;
    },
    dateBuilder() {
      let d=new Date();
      let months=["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days=["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day=days[d.getDay()];
      let date=d.getDate();
      let month=months[d.getMonth()];
      let year=d.getFullYear();
      return `${day} ${month} ${date}, ${year}`;
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
}

#app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0, 0, 0, .25), rgba(0, 0, 0, .75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
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

  box-shadow: 0 0 8px rgba(0, 0, 0, .25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0 20px 0 20px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0 0 16px rgba(0, 0, 0, .25);
  background-color: rgba(255, 255, 255, .75);
  border-radius: 20px 0 20px 0;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  text-shadow: 1px 1px rgba(0, 0, 0, 0.25);
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
  background-color: rgba(255, 255, 255, .3);
  border-radius: 20px;
  margin: 30px 0 10px;

  box-shadow: 3px 6px rgba(0, 0, 0, .25);
}

.weather-box .weather-range-box {
  width: fit-content;
  padding: 10px 18px;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  margin: 20px auto 30px;
}

.weather-box .weather-range-box .temp-range {
  display: inline-block;
  color: #fff;
  font-size: 28px;
  font-weight: 600;
  font-style: italic;
  text-shadow: 2px 2px rgba(0, 0, 0, 0.25);
}

.weather-box .weather-status {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
