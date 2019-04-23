<template>
  <v-card>
    <v-card-title primary-title>
      <h3>{{this.weather.datetime}}</h3>
    </v-card-title>
    <v-card-text>
      <p>Temp: {{this.weather.temp}}</p>
    </v-card-text>
  </v-card>
</template>
<script>
export default {
  name: "forecast",
  data: function() {
      return{
      weather: Object
      }
  },
  props: {
    forecast: Object,
  },
  created(){
      this.weather = this.props.forecast
  },
  watch: {
      weather (newWeather){
          this.weather = newWeather
      }
  }
};
</script>