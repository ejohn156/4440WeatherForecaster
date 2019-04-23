<template>
  <v-card>
    <v-responsive style="background-color: gold" dark>
      <v-layout row>
        <v-flex pa-1>
            <h1>Description: {{weather.weather.description}}</h1>        
          <br>
          <h1>Temperature: {{ Math.floor((weather.temp * 9/5) + 32) }}F</h1>
          <br>
            <h1>Humidity: {{ weather.rh }}%</h1>
            <br>
            <h1>Cloud Coverage: {{ weather.clouds }}%</h1>
            <br>
        </v-flex>
        <v-flex pa-1>
          <h1>Wind Speed: {{weather.wind_spd}}MPH</h1>
          <br>
          <h1>Wind Direction: {{weather.wind_cdir_full}}</h1>
          <br>
          <h1>Visibility: {{weather.vis * 1.60934}}Miles</h1>
          <br>
          <h1>UV: {{weather.uv}}</h1>
          <br>
        </v-flex>
        <!-- <v-flex>
          <giphy style="margin-top:2%;margin-right:2%" v-bind:desc="this.icon"/>
        </v-flex> -->
      </v-layout>
    </v-responsive>
  </v-card>
</template>
<script>
// import giphy from "./giphy";
import axios from "axios"
export default {
  name: "WeatherContent",

  props: {
    weather: Object,
    location: String
  },
  components: {
    // giphy
  },
  data: function(){
      return{
            temp: 0,
            icon: String,
            gradient: 'to top, #FFFF00, #FFD700'
          }
  },
  created(){
    var giphySearch = 'https://api.giphy.com/v1/gifs/search?q='+this.desc+'&api_key=LLtGzlJAN6176Sd5eDEJ6rspOnq1maJo&limit=1'
       
       axios.get(giphySearch).then((response => this.icon = response.data.data[0].images.original.url))
  }
  
};
</script>
<style>
h1{
    color: #1b5e20
}
</style>

