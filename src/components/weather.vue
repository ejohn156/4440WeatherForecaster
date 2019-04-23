
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
                            <div class="text-xs-center" v-if="this.searchType === 'Current'">
                            <h1 class="headline mb-0" style="color:gold">Current Weather in {{ searchedLocation }}</h1>
                            </div>
                            <div class="text-xs-center" v-if="this.searchType === 'Forecast'">
                                <h1 class="headline mb-0" style="color:gold">{{this.forecastArray[selectedForecast].datetime}} Forecast in {{ searchedLocation }}</h1>
                            </div>
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
                    <v-flex pa-6 v-if="this.searchType === 'Current'" style="text-align:center">
                        <WeatherContent v-bind:weather="this.currentWeather[0]" v-bind:location="this.location"/>
                    </v-flex>
                    <v-flex pa-6 v-if="this.searchType === 'Forecast'" style="text-align:center">
                        <!-- <WeatherContent v-bind:weather="this.forecastArray[selectedForecast]" v-bind:location="this.location"/> -->
                        <!-- <v-layout row> -->
                            <h1>This Week</h1>
                            <br>
                            <v-carousel style="margin-top:2%">
                                <v-carousel-item v-for="(page,index) in cardPages" v-bind:key="index">
                                    <!-- component for this weeks forecast carousel -->
                                    <!-- component for next weeks forecast carousel-->
                                    <v-flex v-for="(weather,i) in page" :key="i">
                                    <v-card>
                                        <v-card-title primary-title>
                                            <h3>{{ searchedLocation }} : {{weather.datetime}} : index {{index}}</h3>
                                        </v-card-title>
                                        <v-card-text>
                                            <p>Temp: {{weather.temp}}</p>
                                        </v-card-text>
                                    </v-card>
                                    </v-flex>
                                </v-carousel-item>
                            </v-carousel>
                        <!-- </v-layout> -->
                        <!-- <v-layout row> -->
                            <h1>NextWeek</h1>
                            <v-carousel style="margin-top:2%">
                                <v-carousel-item v-for="(page,index) in nextPages" v-bind:key="index">
                                    <!-- component for this weeks forecast carousel -->
                                    <!-- component for next weeks forecast carousel-->
                                    <v-flex v-for="(weather,i) in page" :key="i">
                                    <v-card>
                                        <v-card-title primary-title>
                                            <h3>{{ searchedLocation }} : {{weather.datetime}} : index {{index}}</h3>
                                        </v-card-title>
                                        <v-card-text>
                                            <p>Temp: {{weather.temp}}</p>
                                        </v-card-text>
                                    </v-card>
                                    </v-flex>
                                </v-carousel-item>
                            </v-carousel>
                    </v-flex>
                        </v-layout>
                    <!-- </v-flex> -->
                    <!-- Forecast Section -->
                <!-- </v-layout> -->
            </v-card>
        </v-container>
        <!-- Simulator Section -->
        <div v-if="this.searchType == 'Current'">
      <sim v-bind:weather="this.currentWeather[0]" />
        </div>
      <div v-if="this.searchType == 'Forecast'">
          <sim v-bind:weather="this.forecastArray[selectedForecast]"/>
      </div>
</div>


</template>

<script>
import lex from "./lex"
import axios from "axios"
import sim from './p5'
import WeatherContent from './weatherContent'
export default {
  name: 'weather',
  components :{
    sim,
    lex,
    WeatherContent
  },
  data: function()  {
    return{
    info: null,
    temp: 0,
    selectedForecast: 0,
    location: "Charlotte, US",
    searchedLocation: "Charlotte, US",
    currentWeather: [],
    forecastArray: [],
    thisWeek: [],
    nextWeek: [],
    forecastLocation: "",
    searchOptions: ["Current", "Forecast"],
    searchMethodOptions: ["City", "Voice"],
    searchType: "Current",
    searchMethod: "City"
    }},
  methods: {
    
    getCurrentWeather: function () {
      this.currentWeather = []
      var currentWeatherAPI = 'https://api.weatherbit.io/v2.0/current?city=' + localStorage.getItem("location") + '&key=86f2b538207d49c7a2168befdb76579a'
      //console.log("current")
      axios.get(currentWeatherAPI).then((response) => {
        //console.log(response.data.data[0])
        this.temp = response.data.data[0].temp
        this.currentWeather.push(response.data.data[0])
      })
    },
    changeCity: function () {
      var currentWeatherAPI = 'https://api.weatherbit.io/v2.0/current?city=' + this.location + '&key=86f2b538207d49c7a2168befdb76579a'
      //spare apikeys: 86f2b538207d49c7a2168befdb76579a, 817a04bb05af4998ba8982692cc8a5ef
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
      this.thisWeek = []
      this.nextWeek = []
      //console.log("forecast")
      this.forecastLocation = localStorage.getItem("location")
      var forecastWeatherAPI = 'https://api.weatherbit.io/v2.0/forecast/daily?city=' + this.forecastLocation + '&key=86f2b538207d49c7a2168befdb76579a'
      axios.get(forecastWeatherAPI).then((response) => {
        var forecastData = response.data.data
        this.location = localStorage.getItem("location")
        var counter = 0
        forecastData.map((weather) => {
          //console.log(weather)
        if(counter > 0 && counter < 15){
          this.forecastArray.push(weather);
        }
          counter++
        })
        
      })
      setTimeout(() => {
            this.populateWeeks()
        }, 200);
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
    populateWeeks(){
        var counter = 0
        this.forecastArray.map((elem)=>{
            if(counter < 7){
            this.thisWeek.push(elem)
            counter++
            }
            else{
                this.nextWeek.push(elem)
                counter++
            }
        })
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

  created() {
    localStorage.setItem("location", "Charlotte, US")
    localStorage.setItem("searchType", "Current")
    this.getWeatherInfo()
    this.getWeatherForecast()
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
},
computed: {
        cardPages () {
          let pageSize = 3
          const pages = []
          for (let i = 0; i < this.thisWeek.length; i += 2) {
              if(i < this.thisWeek.length - 1){
            pages.push(this.thisWeek.slice(i, i + pageSize))
              }
            // if (pageSize < 3) {
            //
            // }
          }
          return pages
        },
        nextPages () {
          let pageSize = 3
          const pages = []
          for (let i = 0; i < this.nextWeek.length; i += 2) {
              if(i < this.nextWeek.length - 1){
            pages.push(this.nextWeek.slice(i, i + pageSize))
              }
            // if (pageSize < 3) {
            //
            // }
          }
          return pages
        }
      }
}
</script>
