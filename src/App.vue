<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm':'' ">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
          >
          <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
            <div class="location-box">
              <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
              <div class="date">{{ dateBuilder() }}</div>
            </div>
            <div class="weather-box">
              <div class="temperature">{{ Math.round(weather.main.temp)}}°C</div>
              <div class="weather">{{weather.weather[0].main}}</div>
            </div>
          </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
    name: 'App',
    data() {
      return {
        api_key: 'de220c90d3df47f91a9841e80eb789b8',
        url_base:'https://api.openweathermap.org/data/2.5/',
        query: '',
        weather: {}
      }
    },
    methods: {
      fetchWeather(e) {
        if (e.key == "Enter") {
          fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
            .then(res => {
              return res.json();
          }).then(this.setResults);
        }
      },
      setResults(results) {
        this.weather = results;
      },
      dateBuilder(){
        let d = new Date();
        let days = ["Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday",];
        let months = ["January","February","March","April","May","June","July","August","September","October","November","December"];

        let day = days[d.getDay()];
        let date = d.getDate();
        let month = months[d.getMonth()];
        let year = d.getFullYear();

        return `${day} ${date} ${month} ${year}`;
      }
    }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

#app{
  background-image: url('./assets/bg1.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm{
  background-image: url('./assets/bg2.jpg');
}

main{
  height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}

.search-box{
  width: 100%;
  margin-bottom: 30px;
  
}

.search-box .search-bar{
  display: block;
  padding: 15px;
  width: 75%;
  align-content: center;

  color: #313131;
  font-size: 20px;

  background: none;
  outline: none;
  border: none;
  appearance: none;

  box-shadow: 0px 0px 8px rgba(0,0,0,0.75);
  background-color: rgba(255,255,255, 0.75);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus{
    box-shadow: 0px 0px 16px rgba(0,0,0,0.75);
    background-color: rgba(255,255,255,0.75);
    border-radius: 16px 0px 16px 0px;

}

.location-box .location{
  margin: 30px;
  color: white;
  font-size: 35px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}

.date{
  color: white;
  font-size: 25px;
  font-style: italic;
  font-weight: 300;
  text-align: center;
}

.weather-box{
  color: white;
  font-size: 35px;
  font-weight: 500;
  text-align: center;
}

.temperature{
  color: white;
  font-size: 100px;
  font-weight: 900;
  font-style: italic;
  margin: 30px 205px ;
  text-align: center;
  background-color: rgba(255,255,255, 0.25);
  border-radius: 16px;
  box-shadow: 3px 6px rgba(0,0,0,0.25);
}

.weather-box .weather{
  color: #fff;
  font-size: 55px;
  font-weight: 700px;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.75);
}
</style>
