
<template>
<div>
    <!-- Navigation Bar -->
  <v-toolbar fixed dark color="#1b5e20">
            <v-toolbar-title class="white--text">4440 Weather Forecaster</v-toolbar-title>

            <v-toolbar-items>
                <v-select style="margin-left: 3%;margin-right: 3%;" v-model="searchMethod" box value="City"
                    v-on:change="onSearchMethodChange" :items="searchMethodOptions">
                </v-select>

                <div v-if="searchMethod == 'City'">
                    <v-text-field style="margin-left: 3%;margin-right: 3%;margin-top:13%" v-model="location" label="Enter A City"></v-text-field>
                </div>
                <div v-if="searchMethod == 'City'">
                    <v-btn style="margin-left: 3%;margin-right: 3%;background-color:gold;color:#1b5e20;margin-top:10%;"
                        v-on:click="changeCity($event)"><b>Get Weather</b></v-btn>
                </div>
                <div v-if="searchMethod == 'Voice'">
                      <lex/>
                </div>
            </v-toolbar-items>
        </v-toolbar>
        <!-- Weather Forecast Content Section -->
        <v-container grid-list-x1 style="margin-top: 5%">
            <v-card style="background-color:#1b5e20;">
                <v-layout row>
                    <v-flex pa-6>
                        <v-card-title primary-title>
                            <h1 class="headline mb-0" style="color:gold">{{ searchedLocation }}</h1>
                        </v-card-title>
                    </v-flex>
                    <v-flex pa-6>
                        <div class="text-xs-center" v-if="this.searchType === 'Current'">
                            <v-chip v-on:click="changeToCurrent" style="background-color:gold">Current</v-chip>
                            <v-chip v-on:click="changeToForecast">Forecast</v-chip>
                        </div>
                        <div class="text-xs-center" v-if="this.searchType === 'Forecast'">
                            <v-chip v-on:click="changeToCurrent">Current</v-chip>
                            <v-chip v-on:click="changeToForecast" style="background-color:gold">Forecast</v-chip>
                        </div>
                    </v-flex>
                </v-layout>

                
                <v-layout row>
                    <!-- Current Weather Section -->
                    <v-flex pa-6 v-if="this.searchType === 'Current'">
                        <current v-bind:currentWeather="this.currentWeather"/>
                    </v-flex>
                    <!-- Forecast Section -->
                    <v-flex pa-6 v-else-if="this.searchType === 'Forecast'">
                        <forecast v-bind:forecastArray="this.forecastArray"/>
                    </v-flex>
                </v-layout>
            </v-card>
        </v-container>
        <!-- Simulator Section -->
      <sim v-bind:currentWeather="this.currentWeather" v-bind:forecastArray="this.forecastArray"/>  
</div>


</template>

<script>
import lex from "./lex"
import axios from "axios"
import sim from './p5'
import current from './current'
import forecast from './forecast'
export default {
  name: 'weather',
  components :{
    sim,
    lex,
    current,
    forecast
  },
  data: function()  {
    return{
    info: null,
    temp: 0,
    location: "Charlotte, US",
    searchedLocation: "Charlotte, US",
    currentWeather: [],
    forecastArray: [],
    forecastLocation: "",
    searchOptions: ["Current", "Forecast"],
    searchMethodOptions: ["City", "Voice"],
    searchType: "Current",
    searchMethod: "City"
    }},
  methods: {
    
    getCurrentWeather: function () {
      this.currentWeather = []
      var currentWeatherAPI = 'https://api.weatherbit.io/v2.0/current?city=' + localStorage.getItem("location") + '&key=817a04bb05af4998ba8982692cc8a5ef'
      //console.log("current")
      axios.get(currentWeatherAPI).then((response) => {
        //console.log(response.data.data[0])
        this.temp = response.data.data[0].temp
        this.currentWeather.push(response.data.data[0])
      })
    },
    changeCity: function () {
      var currentWeatherAPI = 'https://api.weatherbit.io/v2.0/current?city=' + this.location + '&key=817a04bb05af4998ba8982692cc8a5ef'
      
      axios.get(currentWeatherAPI).then((response) => {
        localStorage.setItem("location",response.data.data[0].city_name + ", " + response.data.data[0].country_code)
        this.location = localStorage.getItem("location")
        this.searchedLocation = this.location
        
      })
      if (localStorage.getItem("searchType") == "Current")
        this.getCurrentWeather()
        
      else if (localStorage.getItem("searchType") == "Forecast")
        this.getWeatherForecast()
    },
    getWeatherForecast: function () {
      this.forecastArray = []
      //console.log("forecast")
      this.forecastLocation = localStorage.getItem("location")
      var forecastWeatherAPI = 'https://api.weatherbit.io/v2.0/forecast/daily?city=' + this.forecastLocation + '&key=817a04bb05af4998ba8982692cc8a5ef'
      axios.get(forecastWeatherAPI).then((response) => {
        var forecastData = response.data.data
        this.location = localStorage.getItem("location")
        forecastData.map((weather) => {
          //console.log(weather)

          this.forecastArray.push(weather);
        });
      })
    },
    getWeatherInfo: function () {
      
      if (localStorage.getItem("searchType") === "Current")
        this.getCurrentWeather()
      else if (localStorage.getItem("searchType") === "Forecast")
        this.getWeatherForecast()
    },
    onSearchMethodChange() {
      //console.log(this.searchMethod)
      //localStorage.setItem("searchType", event.target.value)
      

    },
    changeToCurrent(){
      this.searchType = "Current"
      localStorage.setItem("searchType", "Current")
      this.getCurrentWeather()
    },
    changeToForecast(){
      this.searchType = "Forecast"
      localStorage.setItem("searchType", "Forecast")
      this.getWeatherForecast()
    }
  },

  mounted() {
    localStorage.setItem("location", "Charlotte, US")
    localStorage.setItem("searchType", "Current")
    this.getWeatherInfo()
  },
  watch: {
    location(newLocation) {
      localStorage.location = newLocation;
      this.location = newLocation
    },
    searchMethod(newSearchMethod){
      localStorage.searchMethod = newSearchMethod
      this.searchMethodType = newSearchMethod
    },
    searchType(newSearchType){
      localStorage.searchType = newSearchType
      this.searchMethodType = newSearchType
    },
    forecastArray(newForecast){
      localStorage.Forecast = JSON.stringify(newForecast)
      this.searchMethodType = newForecast
    },
    currentWeather(newCurrent){
      localStorage.Current = JSON.stringify(newCurrent)
      this.currentWeather = newCurrent
    }
}
}
</script>
