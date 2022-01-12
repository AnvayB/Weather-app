<template>
<div id = "app" 
    :class="typeof weather.main != 'undefined' && weather.main.temp > 65 ? 'warm' : 'cold' ">
  <main>
    <div class="search-box">
      <input type="text" 
      name="" 
      id="" 
      class="search-bar" 
      placeholder="Search..." 
      v-model="query"
      @keypress="fetchWeather"
      />
    </div>
    <div class="weather-wrap" v-if= "typeof weather.main != 'undefined'">
      <div class="location-box">
        <div class="location">{{ weather.name }},{{ weather.sys.country }}</div>
        <div class="date">{{ dateBuilder() }}</div>
      </div>
      <div class="weather-box">
        <div class="temp">{{Math.round(weather.main.temp)}}ºF</div>
        <div class="weather">
          {{ weather.weather[0].main}} <br> <hr>
          {{ weather.weather[0].description }}
          </div>
      </div>
    </div>
  </main>
</div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: "64be88d0435a2d6eca2e8e9877b67807",
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
    
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=imperial&APPID=${this.api_key}`)
        //callback
          .then(res => {
              return res.json();
            }
          )
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let now = new Date();
      let months = [ 'January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December' ];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[now.getDay()];
      let date = now.getDate();
      let month = months[now.getMonth()];
      let year = now.getFullYear();
      let hour = now.getHours();
      let minutes = now.getUTCMinutes()

      let time = `${hour}:${minutes}`;

      return `${day} - ${month} ${date}, ${year} \n ${time}`;

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
  font-family: 'montserrat', sans-serif;
}

#app {
  background-image: url('./assets/cold-bg.jpeg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app .warm {
  background-image: url('./assets/warm-bg.jpeg');
}

 main {
   min-height: 100vh;
   padding: 25px;

   background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
 }

 .search-box {
   width: 200%;
   margin-bottom: 30px;
 }
 .search-box .search-bar {
   display: block;
   width: 50%;
   padding: 15px;
   color: #313131; 
   /* color: white; */
   font-size: 20px;

   appearance: none;
   border: none;
   outline: none;
   background: none;
   
   box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
   background-color: rgba(255,255,255,0.5);
   border-radius: 0px 16px 0px 16px;
   transition: .4s;
 }
 .search-box .search-bar:focus {
   background-color: rgba(255,255,255,0.75);
   box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
   border-radius: 16px 0px 16px 0px;
 }

 .location-box .location {
   color: white;
   font-size: 32px;
   font-weight: 500;
   text-align: center;
   text-shadow: 1px 3px rgba(0,0,0,0.25);
 }
 .location-box .date {
   color: white;
   font-size: 20px;
   font-weight: 200;
   text-align: center;
   font-style: italic;
 }

 .weather-box {
   text-align: center;
 }
 .weather-box .temp {
   display: inline-block;
   padding: 10px 25px;
   color: white;
   font-size: 102px;
   font-weight: 900;

   text-shadow: 3px 6px rgba(0,0,0,0.25);
   background-color: rgba(255,255,255,0.25);
   border-radius: 16px;
   margin: 30px 0px;

   box-shadow: 3px 6px rgba(0,0,0,0.25);
 }
 .weather-box .weather {
   color: white;
   font-size: 48px;
   font-weight: bold;
   font-style: italic;
   text-shadow: 3px 6px rgba(0,0,0,0.25);
 }
</style>  
