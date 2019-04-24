<template>
  <v-app>
    <weather/>
  </v-app>
</template>

<script>
import weather from './components/weather'
import { components } from 'aws-amplify-vue'
export default {
  name: 'App',
  components: {
    weather,
    components
 }
}

</script>
