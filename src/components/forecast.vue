<template>
  <v-card>
    <v-card-title primary-title>
      <h3>{{forecast.datetime}}</h3>
    </v-card-title>
    <v-card-text>
      <p>Temp: {{forecast.temp}}</p>
    </v-card-text>
  </v-card>
</template>
<script>
export default {
  name: "forecast",
  data: function() {},
  props: {
    forecast: Object
  }
};
</script>