<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp - 273.15 < 16 ? 'cold' : ''">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" placeholder="Search..." 
          v-model="query"
          @keypress="fetchWeather"
          />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != `undefined`">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="data">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">
            {{ Math.round(weather.main.temp - 273.15) }}°C
          </div>
          <div class="weather">
            {{ weather.weather[0].main }}
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data(){
    return {
      api_key: 'd2ac30f2fa095b2d2b4c01baa0bd4ca0',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods:{
    fetchWeather(e){
      if(e.key == 'Enter'){
        fetch(`${this.url_base}weather?q=${this.query}&appid=${this.api_key}`).then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults(results){
      this.weather = results;
    },
    dateBuilder(){
      let d = new Date()
      let months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec']
      let days = ['Sun','Mon','Tue','Wed','Thu','Fri','Sat']
      let day = days[d.getDay()]
      let date = d.getDate()
      let month = months[d.getMonth()]
      let year = d.getFullYear()
      return `${day} ${date} ${month} ${year}`
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
    font-family: 'montserrat', sans-serif;
  }
  #app{
    background-image: url('../assets/hot.jpg');
    background-size: cover;
    background-position: center;
    transition: .4s;
  }
  main{
    display: grid;
    grid-template-columns: 50% 50%;
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0,0,0, .25), rgba(0,0,0,0.6));
  }
  .search-box{
    width: 90%;
  }
  .search-box .search-bar{
    margin-top: 30vh;
    display: block;
    width: 100%;
    padding: 15px;
    color: #353535;
    font-size: 20px;
    border: none;
    background: none;
    outline: none;
    appearance: none;
    box-shadow: 0 0 8px rgba(0,0,0,0.25);
    background-color: rgba(255,255,255,.5);
    border-radius: 0px 16px 0px 16px;
    transition: .4s;
  }
  .search-box .search-bar:focus{
    box-shadow: 0 0 16px rgba(0,0,0,.25);
    background-color: rgba(255,255,255, .75);
    border-radius: 16px 0 16px 0;
  }
  .location-box .location{
    margin-top: 10vh;
    color: white;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0,0,0,.25);
  }
  .location-box{
    color: white;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;
  }
  .weather-box{
    text-align: center;
  }
  .weather-box .temp{
    display: inline-block;
    padding: 10px 25px;
    color: white;
    font-size: 100px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(0,0,0,.25);
    background-color: rgba(255,255,255,.25);
    border-radius: 16px;
    margin: 30px 0;
    box-shadow: 3px 6px rgba(0,0,0,.25);
  }
  .weather-box .weather{
    color: white;
    font-size: 20px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0,0,0,.25);
  }
  #app.warm{
    background-image: url('../assets/hot.jpg');
  }
  #app.cold{
    background-image: url('../assets/cold.jpg');
  }
</style>
