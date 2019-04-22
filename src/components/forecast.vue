<template>
  <v-layout row>
    <v-expansion-panel expand flat class="transparent elevation-0 vuse-expansion">
      <v-flex pa-1 v-for="weather in forecastArray" v-bind:key="weather.datetime">
        <v-card>
          <v-card-title primary-title>
            <h3>{{ searchedLocation }} : {{weather.datetime}}</h3>
          </v-card-title>
          <v-card-text>
            <p>Temp: {{weather.temp}}</p>
          </v-card-text>
        </v-card>
      </v-flex>
    </v-expansion-panel>
  </v-layout>
</template>
<script>
export default {
    name: "forecast",
     props: {
        forecastArray: [Object]
     }
}
</script>