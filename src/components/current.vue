<template>
  <v-card>
    <v-jumbotron v-for="weather in currentWeather" v-bind:key="weather.datetime">
      <v-layout row>
        <v-flex pa-3>
          <h3>Temperature: {{ currentWeather[0].temp }}C</h3>
        </v-flex>
        <v-flex pa-3>
          <h3>Wind Speed: {{currentWeather[0].wind_spd}}MPH</h3>
        </v-flex>
        <v-flex>
            <giphy v-bind:desc="currentWeather[0].weather.description"/>
            </v-flex>
      </v-layout>
    </v-jumbotron>
  </v-card>
</template>
<script>
import giphy from './giphy'
export default {
    name: "current",
     props: {
        currentWeather: [Object]
     },
     components: {
         giphy
     }
}
</script>
