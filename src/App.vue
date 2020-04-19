<template>
  <div id="app" :class="typeof isItHot != 'undefined' && isItHot < 60 ? 'cold' : ''">
    <main>
      <div class="search-box">
        <input class="search-bar" type="text" v-model="searchbox" placeholder="Chiago, Illinois">
        <button id='button' @click='search()'>Get Weather</button>
      </div>

      <div class="weather-wrap">
        <div class="location-box">
          <div class="location">{{searchName}}</div>
          <div class="date">{{currentDate}}</div>
        </div>
      </div>

      <div class="weather-box">
        <div class="temp">{{currentTemp}}</div>
        <div class="weather">{{currentWeather}}</div>
      </div>
    </main>


  </div>
</template>


<script>
  import axios from 'axios';
  let temperature = ''
  export default {
    data() {
      return {
        searchbox: "",
        apikey: process.env.VUE_APP_MY_API_KEY,
        searchName: "",
        currentTemp: "",
        currentWeather: "",
        currentDate: "",
        isItHot: ""
      }
    },
    methods: {
      search: function() {
        let date = new Date()
        date = date.toString().slice(0,15)
        this.currentDate = date
        this.searchbox = this.searchbox.split(',')
        axios
          .get(`/weather?q=${this.searchbox[0]},${this.searchbox[1]}&appid=${this.apikey}&units=imperial`)
          // .get("/weather?q=" + this.searchbox[0] + ',' + this.searchbox[1] + "&appid=" + this.apikey + "&units=imperial")
          .then(response => {
            console.log(response)
            this.currentTemp = Math.round(response.data.main.temp) + '°'
            this.searchName = response.data.name
            this.currentWeather = response.data.weather[0].main
            temperature = response
          }).then(this.isHot)
      },
      isHot: function() {
        this.isItHot = temperature.data.main.temp
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
  background-image: url('https://images.unsplash.com/photo-1531147646552-1eec68116469?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=800&q=60');
  background-size: cover;
  background-position: bottom;
  transition: 0.02s;
  min-height: 100vh;
  padding: 25px;
}

#app.cold {
  background-image: url('https://images.unsplash.com/photo-1491831947735-7f519f52f6db?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=800&q=60');
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
  border:solid 2px grey;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.7);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;

}
.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

#button {
  padding: 10px;
  margin: 10px;
  display: block;
  width: 50%;
  font-size: 20px;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.7);
  border-radius: 0px 16px 0px 16px;
  appearance: none;
  outline: none;
  transition: 0.4s;
  border: solid 2px grey;
  margin-left: 430px;
}

#button:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px; 
}

.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
