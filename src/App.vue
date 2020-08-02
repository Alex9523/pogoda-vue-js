<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Please enter a city in english" 
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>        

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp- 273.15) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>

      </div>
    </main>  
  </div>
</template>



<script>
export default {
  name: 'App',
  mounted:function(){
        this.loadWether() //method1 will execute at pageload
  },
  data () {
    return {
      api_key: 'efd8b5271eba009222f1c2aa184b8ad1',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: 'Vinica',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e){
      if(e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&appid=${this.api_key}`)
          .then(res => {
            return res.json()
          }).then(this.setResults)
      }
    },
    loadWether(){
      fetch(`${this.url_base}weather?q=${this.query}&appid=${this.api_key}`)
          .then(res => {
            return res.json()
          }).then(this.setResults)
      this.query = ''
    },
    setResults(results){
      this.weather = results
    },
    dateBuilder(){
      let d = new Date()
      let months = ['January', 'February', 'March', 'April', 'May', 'June', 'July'
      , 'August', 'September', 'October', 'November', 'December']
      let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'] 

      let day = days[d.getDate()]
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
  width: 500px;
  margin: auto;
  background-image: url("./assets/bg.jpg");
  background-size: cover;
  background-position: center;
  transition: 0.4s;
  }

  main{
    min-height: 100vh;
    padding: 25px;

    background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75 ));
  }

  .search-box{
    width: 100%;
    margin-bottom: 30px;
  }

  .search-box .search-bar{
    display:block;
    width: 100%;
    padding: 15px;
    
    color: #313131;
    font-size: 20px;
    
    appearance: none;
    border: none;
    outline: none;
    background: none;

    box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 0 16px;
    transition: 0.4s;
  }

  .search-box .search-bar:focus{
    box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
    background-color:rgba(225, 225, 225, 0.75);
    border-radius: 16px 0;
  }

  .location-box .location{
    color: #fff;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }

  .location-box .date{
    color: #fff;
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
    color: #fff;
    font-size: 102px;
    font-weight: 900;

    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(225, 225, 225, 0.25);
    border-radius: 16px ;
    margin: 30px 0;

    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }

  .weather-box .weather{
    color:#fff;
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
    text-shadow:3px 6px rgba(0, 0, 0, 0.25);
  }
</style>
