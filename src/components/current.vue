<template>
  <v-card>
    <v-responsive :gradient="gradient" dark>
      <v-layout row>
        <v-flex pa-1>
            <h1>Description: {{currentWeather.weather.description}}</h1>        
          <br>
          <h1>Temperature: {{ Math.floor((currentWeather.temp * 9/5) + 32) }}F</h1>
          <br>
            <h1>Humidity: {{ currentWeather.rh }}%</h1>
            <br>
            <h1>Cloud Coverage: {{ currentWeather.clouds }}%</h1>
            <br>
        </v-flex>
        <v-flex pa-1>
          <h1>Wind Speed: {{currentWeather.wind_spd}}MPH</h1>
          <br>
          <h1>Wind Direction: {{currentWeather.wind_cdir_full}}</h1>
          <br>
          <h1>Visibility: {{currentWeather.vis * 1.60934}}Miles</h1>
          <br>
          <h1>UV: {{currentWeather.uv}}</h1>
          <br>
        </v-flex>
        <v-flex>
          <giphy style="margin-top:2%;margin-right:2%" v-bind:desc="currentWeather.weather.description"/>
        </v-flex>
      </v-layout>
    </v-responsive>
  </v-card>
</template>
<script>
import giphy from "./giphy";
export default {
  name: "current",

  props: {
    currentWeather: Object
  },
  components: {
    giphy
  },
  data: function(){
      return{
            temp: 0,
            icon: String,
            gradient: 'to top, #FFFF00, #FFD700'
          }
  },
};
</script>
<style>
h1{
    color: #1b5e20
}
</style>

