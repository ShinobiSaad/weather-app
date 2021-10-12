<template>
    <div id="app">
      <main>
        <div class="search-box">
          <input 
            type="text" 
            class="search-bar" 
            placeholder="Search the city"
            v-model="query" 
            @keypress="fetchWeather"
            />
        </div>
        <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
          <div class="location-box">
            <div class="location">
              {{ weather.name }}, {{ weather.sys.country }}
            </div>
            <div class="date">
              {{ dateBuilder() }}
            </div>
          </div>
        
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
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
      api_key: '4d572bb4ee7ff2aad3aed56bb90fc2a8',
      url_base: 'http://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then( res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
  dateBuilder () {
    let d = new Date();
    let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
    let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
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
  @import url('https://fonts.googleapis.com/css2?family=Festive&family=PT+Serif:ital@1&display=swap');
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body {
    font-family: 'PT Serif', serif;
    color: antiquewhite;
  }

  #app {
    background: linear-gradient(-45deg, #ee7752, #e73c7e, #23a6d5, #23d5ab);
    background-size: 400% 400%;
    animation: gradient 15s ease infinite;
  }
  @keyframes gradient {
    0% {
        background-position: 0% 50%;
    }
    50% {
        background-position: 100% 50%;
    }
    100% {
        background-position: 0% 50%;
    }
}


  main {
    min-height: 100vh;
    padding: 50px;  
    background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.15), rgba(121, 2, 2, 0.35));
  }

  .search-box {
    width: 100%;
    margin-bottom: 30px;
  }

  .search-box .search-bar {
    font-family: 'PT Serif', serif;
    font-size: 30px;
    display: block;
    width: 100%;
    padding: 15px;
    appearance: none;
    border:none;
    outline: none;
    background: none;
    color: rgb(164, 159, 185);
    background-color: rgb(92, 64, 100);
    border-radius: 20px;
    transition: 0.6s;
    box-shadow: 0 0 16px rgba(48, 40, 70, 0.3);
    
  } 

  .search-box .search-bar:focus {
    background-color:rgb(69, 45, 75);
    border-radius: 50px;
    box-shadow: 0 1px 1px rgb(211, 204, 204);

  }

  .location-box .location {
    color:antiquewhite;
    font-size: 42px;
    font-weight: 500;
    text-align: center;
    text-shadow: 0 1px 1px rgb(15, 11, 11);
  
  }

  .location-box .date {
    color:antiquewhite;
    font-size: 25px;
    font-weight: 500;
    text-align: center;
    
    
  }
  .weather-box {
    text-align: center;
    font-size: 25px;
      
    
  }

  .weather-box .temp {
    display: inline-block;
    padding: 20px 25px;
    margin: 30px 0;
    background-color: rgb(92, 35, 62, 0.25);
    border-radius: 30px;
    font-size: 100px;
    text-shadow: 0 1px 1px rgba(65, 8, 8, 0);
    box-shadow: 0 2px 2px rgb(73, 55, 55);
    font-weight: 800;

  }

  .weather-box .weather {
    color: rgb(209, 168, 168);
    font-size: 40px;
    font-weight: 800;
    text-shadow: 0 1px 1px rgb(15, 11, 11);
  }
</style>
