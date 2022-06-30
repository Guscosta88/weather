<template>
<div class="card">
  <div class="card-body">
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">

        <form @submit.prevent="onSubmit">

        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
        <input type="submit" class="button btn btn-light" value="Submit" />
        </form>
        
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather"><img width="100" v-bind:src="imgBuilder()" /></div>
          <div class="weather">{{ this.weather.weather[0].main }}</div>
          
        </div>
      </div>
    </main>
  </div>
  </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: '4d42b4d6b2f06e6f116936197e278a29',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    onSubmit(){
     fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      },
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    updateCondition(index) {
      this.selectedCondition = index
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
    },
    imgBuilder(){
      let img = this.weather.weather[0].main;

      return require('./assets/images/' + img + '.png');

    }
  }
}
</script>
<style>
  @import './assets/styles/styles.css';

</style>

